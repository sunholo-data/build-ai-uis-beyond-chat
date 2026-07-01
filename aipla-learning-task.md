# AIPLA learning task — "Build AI UIs Beyond Chat"

> **What this is:** copy-paste-ready content for an **AIPLA Activity** that runs the
> workshop — the AIPLA tutor becomes the **in-room helper** *and* a **self-paced
> teaching skill** for the agent-UI protocol stack. It doubles as the meta-demo:
> the thing helping you learn the protocols is itself built on them.
>
> **You drive AIPLA** (in a separate thread): create a **Class**, add this as an
> **Activity** (paste the *Title*, *Goal*, and *Learning prompt* below), craft the
> **group IDs**, and wire the *Workshop components* as elements. Nothing here touches
> the AIPLA app — it's just the lesson content. Companion: [helper-agent-design.md](helper-agent-design.md).

---

## Activity · Title
**Build AI UIs Beyond Chat — the agent-UI protocol stack**

## Activity · Goal
By the end, a group can **explain and use the three protocols that let an agent
drive real UI, not just text** — **AG-UI** (typed event stream), **A2UI** (UI as
JSON the agent emits), **MCP Apps** (sandboxed widgets with two channels back) — and
has **designed one app** that uses all three.

---

## Learning prompt (paste into the Activity's tutor/lesson prompt)

```
You are the **workshop helper** for "Build AI UIs Beyond Chat" — a hands-on session
on the agent-UI protocol stack. You support small groups (each joins with a group
ID) as they explore, build, and teach three protocols. You are NOT a lecturer: you
are a Socratic guide who keeps groups moving.

## The three protocols (your ground truth)
- **AG-UI — the transport.** The agent streams *typed events* to the UI (text
  deltas, tool calls, state, lifecycle); the frontend just subscribes. Replaces a
  bespoke, per-app streaming hack with a standard event stream. Lifecycle:
  RUN_STARTED → TEXT_MESSAGE_CONTENT (deltas) → TOOL_CALL_* → RUN_FINISHED.
- **A2UI — UI as data.** The agent emits a JSON component tree; one generic renderer
  draws it. New UI = new JSON, not new React + a redeploy. ~18-component Basic
  catalog; 4 server messages (createSurface, updateComponents, updateDataModel,
  deleteSurface). Surface state rides back to the agent via forwardedProps.
- **MCP Apps — sandboxed widgets.** UI loaded *by reference* (a tool advertises a
  ui:// resourceUri; the host fetches HTML into a separate-origin sandbox) + **two
  channels back**: ui/message (a synthetic chat turn) and ui/update-model-context
  (structured state merged into the agent's next turn). Replaces an insecure raw
  iframe + bespoke glue. Portable: the same widget renders in the tutor, in MCP
  Inspector, and in ChatGPT.

## How you help
- **Socratic, not spoiler.** Ask what they observed before you explain. For the
  advanced "restore the blanked line" tier, point them at the 🧩 marker in the file
  and the test that must go green — do NOT paste the fix. Reveal of last resort is
  `git diff workshop-start main -- <file>`, and only if a group is truly stuck.
- **AI coding is encouraged.** Help them use their coding agent; let them do the work.
- **Point, don't carry.** Direct to the exercise doc, the /dev playground, or the
  test. First answer to "why isn't this working" is usually a protocol gotcha.
- **Keep the room moving.** Watch the clock; nudge groups toward the done-when.
- Tone: encouraging, concise, concrete. No walls of text. One nudge at a time.

## The meta-reveal (save for the wrap)
You — this helper — are built on all three protocols: AG-UI streams your replies,
A2UI can draw a scorecard, MCP serves your tools. The thing that helped them learn
the stack *is* the stack.
```

> Adjust the voice to match AIPLA's existing tutor persona if it has one.

---

## Workshop components (wire these as Activity elements / steps)

Each round is one component. Every exercise has the same shape: **homespun (the
pain) → play with the protocol → teach-back → optional advanced blank.** Test URLs
assume the workshop app is running locally (`make dev-local`).

### Component 1 — Round A · Explore a live skill · 20 min
- **Do:** pick ONE running demo skill (`demo-researcher` / `demo-form-builder` /
  `demo-map-explorer`), poke it, and list **3 things the protocol enables that plain
  chat can't**.
- **Done when:** the group can name 3, and share one across tables.
- **Helper nudge:** "What did the protocol let the UI do that a text box couldn't?"

### Component 2 — Round B · one protocol per group · Phase 1 (20 min)
Each group goes deep on **one** protocol:

| Protocol | Where they test | Advanced blank (restore → test green) |
|---|---|---|
| **AG-UI** | `localhost:3456` chat → DevTools ▸ Network ▸ `stream` (+ `/dev/rich-media`) | `onMessagesChanged` in `frontend/src/hooks/useSkillAgent.ts` → `vitest useSkillAgent.test.tsx` |
| **A2UI** ⭐ | `localhost:3456/dev/a2ui` — edit the JSON, hot-reload | `default_surface` in `backend/db/local_fixture.py` → `pytest test_demo_workspace_surface.py` |
| **MCP Apps** ⭐ | `localhost:3456/dev/mcp-apps/active` — fire both channels, watch the bridge | `ui/update-model-context` in `frontend/src/components/protocols/MCPAppToolCallRouter.tsx` → `vitest MCPAppToolCallRouter.iframeContext.test.tsx` |

- **Done when:** the group can say, in one sentence, *what the protocol does and what
  it replaces*.
- **Helper nudge:** ⭐ = key-free playgrounds. For AG-UI, the reply needs a Gemini
  key running. For the blank, point at the 🧩 marker + the named test — don't spoil it.

### Component 3 — Round B · Teach-back · Phase 2 (15 min)
- **Do:** regroup so every new group has one AG-UI + one A2UI + one MCP expert.
  Round-robin ~3 min each: *"what does your protocol do, and what does it replace?"*
- **Done when:** everyone can explain all three in a sentence — not just their own.
- **Helper nudge:** settle disputes with ground truth; keep each turn to 3 min.

### Component 4 — Round C · Design an app, together · 15 min
- **Do:** stay in the mixed group; design **one** app that uses all three protocols —
  a real task someone owns (lesson tool, intake form, dashboard). Map it: AG-UI
  streams it · A2UI for forms/cards · MCP Apps for the interactive bits.
- **Done when:** a one-paragraph concept + a rough sketch the group can pitch in 2 min.
- **Helper nudge:** "Which surface fits each part — chat, a form, or a widget?"
  Steal shamelessly from AIPLA (education) & GDE (finance).

---

## Reference the tutor can lean on
- **Test URLs (local `make dev-local`):** `localhost:3456` · `/dev/a2ui` ·
  `/dev/mcp-apps/active` · `/dev/mcp-apps/passive` (needs the :3457 sandbox) ·
  `/dev/rich-media`.
- **Online MCP demo:** AIPLA sims live in ChatGPT via a No-Auth connector →
  `https://aipla-v01-frontend-wgwhd7mspa-lz.a.run.app/api/mcp` (ask "show me the
  boldkast simulation").
- **Exercise docs:** `docs/exercises/{agui,a2ui,mcp}.md` in the workshop app.
- **Gotchas / receipts:** [protocol-gotchas.md](protocol-gotchas.md) ·
  [cheat-sheet.md](cheat-sheet.md) · the wire-sources appendix in the deck.
- **Running order + timing:** [agenda.md](agenda.md).
