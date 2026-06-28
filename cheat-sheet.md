# Cheat sheet — the build rounds at a glance

> One page, key facts only. For the *why* behind each trap, jump to
> [protocol-gotchas.md](protocol-gotchas.md). For where it lives in the code,
> [code-tour.md](code-tour.md).

**Ports:** frontend `3456` · backend `1956` · MCP sandbox `3457`.

---

## AG-UI — the event stream

- **33 typed events, 8 groups** (installed `@ag-ui/core`). The ones you'll touch:
  - `RUN_STARTED` … `RUN_FINISHED` / `RUN_ERROR`
  - `TEXT_MESSAGE_START` → `TEXT_MESSAGE_CONTENT` (deltas) → `TEXT_MESSAGE_END`
  - `TOOL_CALL_START` → `TOOL_CALL_ARGS` → `TOOL_CALL_END` → `TOOL_CALL_RESULT`
  - `STATE_SNAPSHOT` / `STATE_DELTA`
- **Subscribe from the frontend** with `@ag-ui/client` `HttpAgent`:
  `agent.subscribe({ onTextMessageContent, onToolCallStart, onRunFinished, onRunError, … })` — map each event to React state.
- **Per-turn signals go on `forwardedProps`, not `state`.** The wire `state` channel is a backend-output mirror, one turn behind — read it for "what the user just did" and the agent acts on stale data ([gotcha #1](protocol-gotchas.md)).
- **CopilotKit ≠ AG-UI.** Use `@ag-ui/client` directly; don't reach for CopilotKit by reflex ([gotcha #3](protocol-gotchas.md)).

## A2UI — declarative UI

- The agent emits **UI as JSON**; a renderer turns it into real components — **no per-skill React**.
- **Target a surface** via `tool_configs.a2ui.surface_id`: `"chat"` (default), `"workspace"`, `"sidebar"`, `"modal"`, or a custom id.
- **Import the version-pinned entry:** `@a2ui/react/v0_9` + `@a2ui/web_core/v0_9`. The default v0.8 wrapper silently drops v0.9 specs ([gotcha #5](protocol-gotchas.md)).
- v0.9 **renamed the catalog "Standard" → "Basic"** ([gotcha #6](protocol-gotchas.md)).
- Surface state rides **back** to the agent on `forwardedProps.a2ui_surface_state` — the bidirectional loop.

## MCP Apps — sandboxed widgets

- **UI-by-REFERENCE:** `tools/list` declares `_meta.ui.resourceUri`; `tools/call` returns **data only**; the host **separately** calls `resources/read` for the HTML ([gotcha #7](protocol-gotchas.md)).
- Use `@mcp-ui/client@7` **`AppRenderer`** (renamed from `UIResourceRenderer`, [gotcha #8](protocol-gotchas.md)).
- **Two iframe→host channels — wire both** ([gotcha #9](protocol-gotchas.md)):
  - `ui/message` — synthetic chat turns
  - `ui/update-model-context` — structured state merged into the agent's next turn
- Sandbox **must be a separate origin** (dev: port `3457`) ([gotcha #10](protocol-gotchas.md)). The backend MCP proxy must declare **GET + POST + DELETE** ([gotcha #11](protocol-gotchas.md)).

## ADK / skills

- **Skills are DATA** (a `SkillConfig`), not code. `model=` accepts `gemini` / `claude` / `gpt` — no per-skill provider code.
- **Wrapping tools:**
  - Internal Python, or a third-party HTTP API → `FunctionTool` (in-process).
  - A service that already speaks MCP → `tool_configs.mcp.servers` / `McpToolset`.
  - **Don't wrap your own Python as an MCP server** ([gotcha #12](protocol-gotchas.md)).

---

> **Before you debug, check [protocol-gotchas.md](protocol-gotchas.md)** — odds are it's catalogued. For where each piece lives in the codebase, see [code-tour.md](code-tour.md).
