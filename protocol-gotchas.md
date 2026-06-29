# Protocol gotchas — the bear traps we found, so you don't have to

> The new agent UI protocols (AG-UI, A2UI, MCP Apps) are young. The specs
> are clear on paper; the SDKs are at v0.x. Most things just work. A few
> don't — and those few cost real days of debugging when you hit them
> blind. This page captures every protocol-layer trap we hit during the
> v6 bring-up so you can route around them.

Each entry links to the workshop block where the trap most often
appears, plus a one-line fix and the design doc / commit that proves
the fix works.

## How to use this page

- **Reading the workshop agenda?** Glance at the "block N" column to
  see which traps lie ahead.
- **Building your own skill in block 5?** Search Cmd-F for the
  framework you're using (AG-UI, A2UI, MCP, CopilotKit).
- **Hit something weird?** Check here before opening DevTools — odds
  are it's catalogued.

---

## AG-UI traps (block 2)

### 1. Wire `state` is **one turn behind**

**Symptom:** Frontend sends `forwardedProps.document_ids = [A, B]` after
the user opens doc B. Backend reads `state.document_ids = [A]` from the
prior turn's `STATE_SNAPSHOT` and ignores the fresh signal. Agent denies
doc B exists.

**Why:** `HttpAgent.prepareRunAgentInput` puts `state: this.state` in
the body, where `this.state` is updated **by `STATE_SNAPSHOT` events
the backend emits**. That makes the wire `state` channel a
*backend-output mirror*, round-tripped one turn late. Perfect for
resumption hints, lethal for "what did the user just do."

**Fix:** Per-turn signals belong on `forwardedProps`. Parser priority:
`forwardedProps > top-level body > state`. The bare-key state write
keeps round-tripping; the parser just refuses to read the round-trip.

**Sub-trap:** Don't reach for the `temp:` prefix to make the round-trip
"go away." ADK's `temp:` semantics are in-invocation only —
`base_session_service._trim_temp_delta_state` strips temp keys from
`event.actions.state_delta` *before* persistence; `ag_ui_adk` then
re-fetches the session, so the temp value lives only on a transient
copy that gets garbage-collected. By the time your callback runs,
`temp:document_ids` is gone.

### 2. Own the **full** AG-UI boundary

**Symptom:** You "adopted AG-UI" — you use `ag_ui_adk` on the backend.
But your HTTP endpoint takes a custom `{message: str}` body. Every call
from `HttpAgent` results in a `RUN_ERROR` in the frontend — no stack
trace, no 4xx, just a 200 OK with an error event in the SSE stream.

**Why:** `HttpAgent` sends the AG-UI wire format:
`{messages: [...], threadId, state, forwardedProps}`. Pydantic
*silently drops extra fields* — your custom `{message: str}` body sees
`message = ""`, ADK errors on empty input, the error gets serialised as
a streaming `RUN_ERROR` event, which the frontend treats as a normal
agent failure.

**Fix:** Accept `RunAgentInput` at the HTTP layer; let `ag_ui_adk`
consume it directly. Half-adopting a protocol breaks silently —
*half-adoption is worse than no adoption* because the failure mode is
invisible.

### 3. **CopilotKit ≠ AG-UI** (don't reach for CopilotKit by reflex)

**Symptom:** You install `@copilotkit/react-ui` to get the chat UI.
Nothing wired correctly. Your `<CopilotKit>` provider needs a
`runtimeUrl`, but you've got a plain AG-UI backend at `/api/proxy/...`.

**Why:** `@copilotkit/react-*` speaks CopilotKit's *own* GraphQL
runtime, not AG-UI SSE. Internally CopilotKit's runtime uses AG-UI to
talk to its backend adapters — but the React side is its own thing.

**Fix:** Use `@ag-ui/client.HttpAgent` directly. v6 has 0 lines of
CopilotKit code; the entire chat UI is `agent.subscribe({...})` in
[`useSkillAgent.ts`](useSkillAgent.ts) (see [`code-tour.md`](code-tour.md)).

If you want the polished CopilotKit chrome (sidebar, popup, themes),
you can still do that — but adopt the full CopilotKit stack
end-to-end (runtime + adapters), don't try to half-marry it to a
bare-AG-UI backend.

### 4. Tool-only assistant turns can render blank

**Symptom:** You emit a tool call that returns A2UI JSON. The workspace
pane updates, but the chat bubble that hosted the tool-call indicator
is empty — no spinner, no "the agent called X" badge, nothing.

**Why:** `useSkillAgent.toSkillMessage` was treating
`content: undefined` (which is what assistant messages with only tool
calls have) as "no message," and skipping the render entirely. The
dispatcher attached to that bubble never mounted, so the surface
update arrived with no host to mount in.

**Fix:** Coerce `content: undefined` to `""` for assistant messages.
The bubble renders even when empty — it just becomes a host for tool
indicators and inline UI.

---

## A2UI traps (block 3)

### 5. The SDK wrapper synthesizes v0.8 messages internally

**Symptom:** Your skill emits perfectly spec-compliant v0.9 A2UI
specs (the LLM has the v0.9 schema from `render_as_llm_instructions()`),
your backend validates against v0.9, but the workspace pane stays empty.
No errors in the console. The renderer just silently drops the spec.

**Why:** `@a2ui/react`'s default entry point is the v0.8 wrapper —
`A2UIViewer({root, components, data})` synthesizes v0.8
`{beginRendering, surfaceUpdate, dataModelUpdate}` messages internally.
When the spec is v0.9 shape (`{version, createSurface, updateComponents,
updateDataModel}`), `isStaticSpec` returns false, the wrapper's
dispatcher silently drops the message.

**Fix:** Import from the version-pinned entry — `@a2ui/react/v0_9` +
`@a2ui/web_core/v0_9` — and use `MessageProcessor` + `<A2uiSurface>`
directly. v6 has one `MessageProcessor` per surface, auto-`createSurface`
to handle LLM message-ordering drift, idempotent on tool-call id.

### 6. v0.9 renamed "Standard" → **"Basic"**

**Symptom:** Your skill prompt asks the LLM to "use the Standard
catalog." LLM emits valid v0.9, but the components fail validation.

**Why:** Alpha-API churn. v0.9 renamed the canonical catalog. Backend
constant is now `BasicCatalog`; `CatalogConfig.from_path()` for custom
catalogs.

**Fix:** Update skill prompts. Better: the SDK auto-injects the schema
via `render_as_llm_instructions()` — don't hand-write the catalog name
into prompts in the first place.

---

## MCP Apps traps (block 4)

### 7. MCP Apps is UI-by-**REFERENCE**, not embedded

**Symptom:** Your router inspects the `tools/call` result looking for
an embedded UI resource. It's not there.

**Why:** The MCP Apps spec is UI-by-reference:
- `tools/list` declares `_meta.ui.resourceUri = "ui://server/widget.html"`
  on each UI-bearing tool.
- `tools/call` returns ONLY data (plain text + `_meta.viewUUID`); NO
  embedded UI resource.
- The host **separately** calls `resources/read(uri)` to fetch the
  HTML.

**Fix:** Renderer needs **both** the tool definition (with
`_meta.ui.resourceUri`) AND either an MCP `Client` (to fetch the
resource) or pre-fetched `html`. `@mcp-ui/client@7.0.0`'s `AppRenderer`
makes this explicit; older mental models from blog posts are stale.

### 8. `@mcp-ui/client@7.0.0` is **`AppRenderer`** (not `UIResourceRenderer`)

v7 rename. Older blog posts and docs reference `UIResourceRenderer`.
Use `AppRenderer`. Same component, different name.

### 9. **Two** iframe→host RPC channels, not one

**Symptom:** Cesium map demo shows Munich correctly when the agent
calls `show-map`. User asks "what city is currently centred?" — agent
calls `show-map` again instead of answering from context. The agent has
no idea what's on screen.

**Why:** MCP Apps defines **two** distinct iframe→host RPC methods:
- `ui/message` — synthetic chat turns ("I clicked Munich" → goes into
  the next agent turn as if the user said it)
- `ui/update-model-context` — structured state (current map bounds,
  selected city) merged into the agent's NEXT-turn context under
  `mcp_app_context.{server}.{tool}`

Without an `onUpdateModelContext` handler, the iframe gets
`MCP error -32601: No handler for method`, the map still renders, but
the agent stays blind to its own UI.

**Fix:** Wire both. In `@mcp-ui/client@7.0.0`, `ui/update-model-context`
surfaces via the catch-all `onFallbackRequest` callback (dispatch on
`request.method`), not a dedicated prop. Backend needs a parallel
endpoint (v6: `POST /api/sessions/{id}/iframe-context`) that merges the
update into session state for the next turn. **Seven access gates** on
that endpoint — see [`code-tour.md`](code-tour.md) #7.

### 10. The sandbox **must** be a separate origin

**Symptom:** Inner iframe with `allow-same-origin` reads host cookies
+ Firebase tokens.

**Why:** That's how same-origin works. The sandbox iframe is
defence-in-depth; it has to be on a different origin from the host
shell or `allow-same-origin` defeats the whole point.

**Fix:** v6 ships an
[`infrastructure/mcp-sandbox/`](https://github.com/sunholo-data/ai-protocol-platform/tree/main/infrastructure/mcp-sandbox) — tiny Express server on port 3457
in dev, separate Cloud Run service in deployed envs. CSP is set via HTTP
headers (tamper-proof; meta-tag CSP can be modified by served HTML).
Referrer + origin validation on every postMessage. Sprint 2.13's
artefact-review hook (see [`code-tour.md`](code-tour.md) #7) sits
*above* this — defence-in-depth, not replacement.

### 11. MCP TS SDK opens GET + POST + DELETE on streamable_http

**Symptom:** You set up a backend proxy that only handles POST for the
MCP server. Client aborts every call with `net::ERR_ABORTED`. No useful
error message.

**Why:** `StreamableHTTPClientTransport` opens a long-lived GET (the
SSE channel) alongside POSTs (requests). The SDK aborts the whole
session if the GET 404s. DELETE is used for session teardown.

**Fix:** Backend proxy declares GET + POST + DELETE handlers, all gated
on the same auth + allowlist. v6's [`mcp_proxy.py`](https://github.com/sunholo-data/ai-protocol-platform/blob/main/backend/protocols/mcp_proxy.py)
([`code-tour.md`](code-tour.md) #7) does this — read the route
declarations at the top.

---

## ADK + general traps (block 5)

### 12. Don't wrap your own Python as an MCP server

**Symptom:** You're tempted to expose `summarise_url(url)` as an MCP
server. Now you have a separate process, a separate transport, and
JSON-RPC overhead — for code you wrote yourself, that you could call
directly.

**Why:** MCP is for *external* tools (third-party APIs, vendor
integrations, things the agent shouldn't need source for). ADK
`FunctionTool` runs in-process — no transport, no serialisation, sub-ms
overhead.

**Fix:**
- **Internal Python you wrote?** Wrap as `FunctionTool`.
- **Third-party API you call via HTTP?** Wrap as `FunctionTool`.
- **Genuinely external service that already speaks MCP?** Use `McpToolset`
  (or `tool_configs.mcp.servers` in your `SkillConfig`).

### 13. Next.js `/api/proxy/[...path]` strips the prefix silently

**Symptom:** Your backend declares a route at `prefix="/api/proxy/mcp"`.
You curl it from the laptop — works. You navigate in-browser through
the Next.js frontend — 404.

**Why:** Next.js's catch-all at `/api/proxy/[...path]` strips
`/api/proxy/` and forwards `[...path]` to the backend. So
`/api/proxy/mcp/X` → Next strips → backend sees `/mcp/X`. Your declared
route never matches.

**Fix:** Backend prefix is just `/mcp` (no `/api/proxy/`). The curl-
from-laptop test that "worked" was misleading — it bypassed Next.js.
Test through the same path the browser uses.

---

## MCP Apps cross-client rendering (the demo clients)

> Newer field notes — from putting the portable AIPLA sims
> (`show_boldkast`/`show_kinebot`/`show_led_planck`) in front of real
> consumer hosts in **June 2026**, not from the v6 bring-up. Verified
> against `external-host-demo/smoke_test.py` + a live MCP Inspector
> render. They bite at the **MCP Apps demo** (the tour) — which is exactly
> why the workshop drives that demo through MCP Inspector, not a consumer
> app.

**Spec provenance:** MCP Apps is the first official MCP extension
(`io.modelcontextprotocol/ui`, SEP-1865) — a joint Anthropic + OpenAI +
MCP-UI spec, proposed 21 Nov 2025, launched **26 Jan 2026**.

**Who actually renders MCP App UI** — official
[client matrix](https://modelcontextprotocol.io/extensions/client-matrix),
mid-2026, **11 hosts**: Claude (web + Desktop) · ChatGPT · VS Code (GitHub
Copilot) · Microsoft 365 Copilot · Goose · Postman · MCPJam · Cursor ·
Archestra.AI · PostHog Code. Plus **MCP Inspector** — the reference renderer
(an *Apps* tab; not in the end-user matrix but verified live). **Not
renderers:** **Slack** (Block Kit, not iframes — see #19); and simply not
listed as of mid-2026: Windsurf, Zed, Cline, Continue, Jan, Open WebUI,
LibreChat. **The caveat that matters:** a matrix CHECK records *declared*
support at the init handshake, **not** verified mounting — Claude Desktop
(#14) is the cautionary tale.

### 14. Claude Desktop calls the tool but renders **text, not the widget**

**Symptom:** In Claude Desktop (build `1.15962.0`), "show me the boldkast
simulation" calls `aipla-sims show_boldkast` successfully and prints the
tool's text — including the server's own fallback line *"[This tool call
rendered an interactive widget in the chat…]"* — but **no interactive
widget mounts**. Just text.

**Why:** Early-2026 Claude Desktop builds *negotiate* MCP Apps and call
the tool, but don't mount the `ui://` resource as an iframe. Host-side
gap, not a server bug — the server returned a spec-compliant MCP App
(`smoke_test.py`: valid `text/html;profile=mcp-app` + bridge), and the
**same** `show_boldkast` mounts the live sim in MCP Inspector v0.22's
*Apps* tab. Corroborated upstream: ext-apps issues **#671** and **#615**
document claude.ai / Claude Desktop negotiating the `ui` capability and
fetching the resource but failing to mount the iframe.

**Fix / the tell:** The tool's text content with **no embedded panel**
*is* the text fallback — the host didn't mount the UI. Don't debug the
sim or the config (the call succeeded → both are fine); it's the host.
Demo via **MCP Inspector** or the AIPLA tutor instead.

### 15. MCP Inspector is the reference renderer — use it as ground truth

**Symptom:** A consumer host shows text and you can't tell whose fault it
is — the sim, the config, or the host.

**Why / fix:** `npx @modelcontextprotocol/inspector uv run --script
server.py` launches the reference renderer locally — no auth, no cloud,
no tunnel. v0.22 has an **Apps** tab that lists the `show_*` tools and
mounts them (sliders move, readout updates). If Inspector mounts it, the
sim is spec-correct and the gap is **host-side**; if Inspector *also*
fails, it's the server's `_meta`/resource shape. One step splits the
problem. In a Codespace, VS Code auto-forwards port `6274` to a private
HTTPS URL — same render, inside the workshop environment, no install.

### 16. A deployed `/mcp` endpoint ≠ your sims rendering there

**Symptom:** You point a remote client (ChatGPT connector, remote
Inspector) at the deployed `…/api/proxy/mcp`, expecting the physics sims.
You get text-returning **skill** tools — or a `502` — and no
`show_boldkast`, no `ui://` resource.

**Why:** The platform's mounted server
(`backend/protocols/mcp_server.py` → `get_mcp_asgi_app()` →
`rebuild_tools()`) registers **one tool per public marketplace skill** via
`_make_skill_tool`, and those return concatenated **text**. The sims'
`show_*` tools + `ui://` resources live **only** in the standalone
`external-host-demo/server.py`. (Observed live: the proxy returned `502
backend_unreachable` — Cloud Run scale-to-zero.)

**Fix:** To serve the sims from the cloud URL — fold the `artefacts/`
registration into `mcp_server.py`, redeploy, confirm `/api/proxy/mcp`
passes streamable-HTTP cleanly, and **decide auth** (the endpoint is
open; the sims assume the AIPLA tutor). Until then, run Inspector
locally. "Has an MCP endpoint" never implies "renders these widgets."

### 17. ChatGPT needs a remote server + HTTPS tunnel + Developer mode

**Symptom:** You want the sims in ChatGPT but there's no "add a local
server" option.

**Why:** ChatGPT connectors reach **remote** servers over HTTPS only — a
stdio/localhost server won't do. You need the `--http` server + a public
HTTPS tunnel (cloudflared/ngrok) + org-enabled **Developer mode** + a
hand-created connector pointing at `https://<tunnel>/mcp`.

**Fix / implication:** Tunnel- and wifi-dependent → fine for a pre-staged
"it works in ChatGPT too" beat, risky live. For a live room, Inspector
(local) is the reliable route; once the deployed URL serves the sims
(#16), point the ChatGPT connector at the cloud URL and skip the tunnel.
(Bonus tell: a Claude-Desktop-spawned MCP server is **stdio** — it has no
TCP port, so "is it running?" means checking the process tree, not
`lsof`.)

### 18. Hosts agree on the render, disagree on the **interaction→model back-channel**

**Symptom:** The same sim mounts in ChatGPT's connector and looks perfect,
but when you drag a slider or play, the model has no idea — *"it doesn't
see what I interact with."* In the AIPLA app (and Inspector) those same
interactions **do** reach the model (the tutor reacts: *"Fedt, du har
prøvet at køre simulationen!"*).

**Why:** The **render** path has converged — UI-by-reference
(`_meta.ui.resourceUri` → `resources/read`) is shared between MCP Apps and
OpenAI's Apps SDK, so the iframe shows up everywhere. The **back-channel is
subtler than it first looks** (and we got it wrong on the first pass).
OpenAI's own docs say to *"build with the MCP-standard keys by default"* —
`ui/update-model-context` — and `window.openai` (`setWidgetState` /
`sendFollowUpMessage`) is an **additive** layer that *maps onto* the
standard, **not** a replacement. So ChatGPT *does* claim standard support.
**But in practice it's unreliable for a standard-only widget:** our sim
(standard-only, no `window.openai`, `boldkast/v1/index.html:639–713`)
rendered yet the model stayed blind, and OpenAI has a documented
**June-2026 stale-context bug** (`openai-apps-sdk-examples#221`) where
`ui/update-model-context` is sent but later turns read a stale value.
Whether a standard-only widget feeds back reliably in production ChatGPT is
an **open question the MCP/OpenAI docs leave unsettled**. MCP-standard hosts
(the AIPLA app, MCP Inspector) consume the channel reliably.

**Fix / implication:** For a sim that talks back *reliably everywhere*, the
bridge should **dual-speak** — emit `ui/update-model-context` (the standard,
for the AIPLA app / Inspector / spec-pure hosts) **and** `window.openai.*`
(for robust ChatGPT behaviour). For the workshop: demo the **full loop** in
the AIPLA app or Inspector; in ChatGPT, treat it as **render-first** and
don't bank on the model seeing interactions. Inspector's *Server
Notifications* panel shows the sim correctly emitting `ui/update-model-context`
— proof the gap is host-side. **Teaching point:** the View standardized
faster than the interaction loop — "portable UI" is real today, "portable
bidirectional context" is still settling.

### 19. Slack renders MCP App widgets? No — it speaks Block Kit, not iframes

**Symptom:** You assume the sim will mount in Slack like it does in ChatGPT.
It won't.

**Why:** Slack's surface is **Block Kit** (cards, data tables, carousels,
alerts, work objects) rendered *natively* — not sandboxed HTML, not `ui://`
resources, not `text/html;profile=mcp-app`. Slack is **absent from the MCP
Apps client matrix** entirely; even Slack's MCP *client* translates partner
output **to Block Kit** (`slack://blocks/…`) rather than mounting an MCP App
iframe, and the "Slack MCP Server" is a tool/data **gateway** (a server
role), not a UI host.

**Fix / implication:** There's no "render this MCP App in Slack" — you'd
rebuild the UI as Block Kit (separate, non-portable). For the workshop, Slack
is the clean illustration that **"supports MCP" ≠ "renders MCP Apps"**: it
speaks the protocol for *tools* but is not an Apps *host*.

---

## The back-channel transport gap (block 3 + 4)

> The deepest "young protocol" tell. The *render* path converged across the
> ecosystem; the *interaction → model* back-channel did not — so the two
> protocols we ship reach the agent over **different transports**.

### 20. A2UI rides AG-UI; MCP Apps **can't** — and that's not a bug

**Symptom:** A2UI surface state reaches the agent via AG-UI
`forwardedProps.a2ui_surface_state` (and it works). You assume an MCP App
widget's state arrives the same way, go looking for it on the AG-UI run
input — and it's never there. The agent reads it from
`session.state["mcp_app_context.*"]` instead.

**Why:** the renderer's **owner** dictates the transport.
- **A2UI** surfaces are **host-rendered, same-origin** — the host owns the
  `SurfaceModel`, so it snapshots the live `dataModel` at `sendMessage` and
  attaches it to AG-UI `forwardedProps`. Rides the AG-UI run input.
- **MCP Apps** widgets are **sandboxed, cross-origin iframes** (`:3457`, no
  `allow-same-origin`). They speak **MCP-UI postMessage (JSON-RPC)**, not
  AG-UI, and the host can't read inside them. The widget must *push* state
  via the spec method `ui/update-model-context`; the host stashes it
  (`POST /api/sessions/{id}/iframe-context` → `session.state`) for the next
  turn. It never touches AG-UI — **by design**, because MCP Apps must run in
  hosts that have no AG-UI at all (Claude Desktop, ChatGPT, …).

Both then converge: an ADK `InstructionProvider` injects a namespaced,
framed block into the **next system prompt** (`a2ui_surface_context.*` /
`mcp_app_context.*`) — read as context, never a tool call.

**Fix / implication:** Don't try to force MCP App context onto
`forwardedProps` — the sandbox makes it impossible, and host-portability is
the whole point. This asymmetry is **protocol immaturity, not a template
bug**: the View standardized faster than the interaction loop (see #18).
Same destination (the prompt), two roads — dictated by who owns the
renderer. Source: the two `wrap_with_*` InstructionProviders
(`backend/adk/a2ui_surface_context.py`, `backend/adk/iframe_context.py`).

---

## Where these came from

- [`docs/talks/ai-ui-protocol-stack.md`](https://github.com/sunholo-data/ai-protocol-platform/blob/main/docs/talks/ai-ui-protocol-stack.md) — the living
  verification log + anti-patterns list. Every entry here is dated and
  cites the commit / smoke test that proved the fix.
- The platform's [`docs/design/v6.X.Y/implemented/`](https://github.com/sunholo-data/ai-protocol-platform/tree/main/docs/design) sprint docs — each fix
  has a design doc with the worked example.
- **#14–19 (MCP Apps cross-client)** — June 2026 live testing (Claude
  Desktop, ChatGPT, MCP Inspector) + a verified research pass over primary
  sources: the [MCP Apps announcement](https://blog.modelcontextprotocol.io/posts/2026-01-26-mcp-apps/),
  the [client matrix](https://modelcontextprotocol.io/extensions/client-matrix),
  [OpenAI Apps SDK docs](https://developers.openai.com/apps-sdk/), Slack's
  developer docs, and ext-apps issues #671 / #615 / openai-apps-sdk-examples #221.
- **#20 (back-channel transport)** — the two `wrap_with_*` InstructionProviders
  (`backend/adk/a2ui_surface_context.py`, `backend/adk/iframe_context.py`) and the
  sprint docs `a2ui-surface-context.md` (v6.2.0) / `mcp-app-update-model-context.md`
  (v6.1.0), cross-checked against the talk-doc verification log.

The talk doc is the **source of truth**. This page is the workshop
distillation — protocol traps only, no IaC/deploy gotchas, mapped to
agenda blocks.

## After the workshop

If you hit a new trap building your own skill, send it back — open a
PR against `docs/talks/ai-ui-protocol-stack.md` in the platform repo
adding a row to the verification log. The talk's credibility (and this
page's) comes from being field notes, not theory.
