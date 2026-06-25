# Workshop fork — scope, exercises & solution-rating

The activity-led workshop needs a **runnable, pinned fork of the platform** —
the one place the Round-B reconstruct exercises can live (they're blanked-out
*code*, not markdown). This doc scopes that fork: what it is, how start↔solution
works, exactly what gets blanked per track, and how attendees upload solutions
for the helper agent to rate.

## Why a fork (not the live template, not this repo)

- **Exercises are code.** Round B blanks real lines in the platform's frontend/
  backend; that has to be a fork of [`ai-protocol-platform`](https://github.com/sunholo-data/ai-protocol-platform), not this Markdown repo.
- **Pinned & tested.** The public template keeps evolving. The cohort should run
  one identical, known-good snapshot — so a green `make dev-local` on the
  instructor's laptop guarantees green on everyone's.
- **Pre-seeded.** The fork ships the demo skills, the helper-agent skill, and the
  anonymous-group join already configured, so nobody burns setup time on them.

## Proposed repo

| | |
|---|---|
| **Name** | `sunholo-data/build-ai-uis-workshop-app` (matches this materials repo) |
| **Forked from** | `sunholo-data/ai-protocol-platform` @ a pinned tag (e.g. `workshop-2026-08`) |
| **Attendees clone** | this fork (pre-work changes from "clone the template" → "clone the workshop app") |
| **License / lineage** | unchanged from the template (Apache-2.0) |

> Name is a proposal — easy to change. Everything below refers to it as "the fork."

## Start ↔ solution mechanism

**One `workshop-start` branch holds all three blanked spots; `main` holds the
complete solution.** Rationale: jigsaw groups each touch only *their* file, so a
single start branch is simplest to run and clone.

```
main             ← complete, working (the solutions)
workshop-start   ← all three exercises blanked; attendees work here
```

- Pre-work clones and checks out `workshop-start`.
- Each blanked spot carries a visible marker + a pointer to its solution:
  ```js
  // 🧩 WORKSHOP EXERCISE (AG-UI) — restore this handler. See docs/exercises/agui.md
  // Solution: `git diff workshop-start main -- <this file>`
  ```
- A stuck group peeks with `git diff workshop-start main -- <file>` (or the helper
  agent reveals on request). No separate solution repo to keep in sync.
- `docs/exercises/{agui,a2ui,mcp}.md` in the fork = the same content as the
  Round-B activity cards in the deck (single source; the cards link here).

## The three reconstruct exercises (Round B jigsaw)

Each is **one focused, observable edit** — restore a deleted handler/value, then
confirm a visible behaviour. Difficulty is comparable so groups finish together.

> ⚠️ **Build note:** exact files/lines below are from the code-tour and must be
> confirmed against the fork's actual source when authoring the blanks. The
> *behaviour to restore* and the *success check* are the stable contract.

### B1 · AG-UI — restore the event stream
- **File:** `frontend/src/hooks/useSkillAgent.ts` (the `agent.subscribe({…})` block)
- **Blanked:** the `onTextMessageContent` handler that appends each delta to the
  current assistant message.
- **Restore:** append `event.delta` to the in-progress message so text streams.
- **✅ Success:** messages render token-by-token; events visible in DevTools →
  Network → `stream`.
- **Solution:** [`useSkillAgent.ts` on the template](https://github.com/sunholo-data/ai-protocol-platform/blob/main/frontend/src/hooks/useSkillAgent.ts)

### B2 · A2UI — send UI to the workspace surface
- **File:** the `demo-form-builder` / `demo-workspace` skill config in
  `backend/db/local_fixture.py` (`tool_configs.a2ui`).
- **Blanked:** `surface_id` (the UI has no declared surface, so it falls back to
  the chat bubble).
- **Restore:** `tool_configs.a2ui.surface_id = "workspace"`.
- **✅ Success:** "make me a contact form" renders in the **workspace pane**, not
  inline in chat.
- **Gotcha to surface:** the v0.9-vs-v0.8 import trap (protocol-gotchas #5) — good
  stretch if a group finishes early.

### B3 · MCP Apps — wire the second RPC channel
- **File:** the frontend MCP-app renderer (the `onFallbackRequest` /
  `onUpdateModelContext` dispatch) — paired with `backend/protocols/mcp_proxy.py`
  / `POST /api/sessions/{id}/iframe-context`.
- **Blanked:** the `onUpdateModelContext` handler, so iframe state never reaches
  the next turn.
- **Restore:** dispatch `ui/update-model-context` so the widget's state merges
  into the agent's next-turn context.
- **✅ Success:** show a map, ask "what city is currently centred?" — the agent
  answers from context with **no re-render**.
- **Solution:** [`mcp_proxy.py` on the template](https://github.com/sunholo-data/ai-protocol-platform/blob/main/backend/protocols/mcp_proxy.py)

## Round C — the skeleton skill (prototype starter)

A single copy-and-modify file (the long-promised `skeleton-skill.md` made real in
the fork): one `SkillConfig` with `instructions`, `model`, one `FunctionTool`
stub, and commented `tool_configs` blocks for `a2ui` and `mcp`. Groups fork it,
change the instruction + one tool, and they have a running skill. AI coding
encouraged. Pairs with [`prototype-canvas.md`](prototype-canvas.md) (the planning
worksheet).

## Upload solutions → the helper agent rates them

The headline new asset, and the best meta-demo we have: attendees submit work and
the **helper agent grades it live**, using every protocol it's teaching.

### Flow
1. Group finishes a Round-B reconstruct (or a Round-C prototype).
2. They submit via the helper agent — either:
   - **paste a diff / snippet** into chat, or
   - **share a branch/PR link** on their fork.
3. The agent calls a `rate_submission(track, payload)` tool → **LLM-as-judge**
   against a per-track rubric (+ the reference solution for Round B).
4. Result renders as an **A2UI scorecard** in the workspace pane (score per
   dimension + one concrete next step), and the run **streams** the critique via
   AG-UI as it thinks.
5. Submissions accumulate into a **live gallery / leaderboard** in the workspace
   (show-and-tell momentum, no slide-flipping).

### Rubric (per track, 0–3 each)
- **Works** — does the success check pass? (objective for Round B)
- **Correct** — matches the reference behaviour / spec (Round B) or is technically
  sound (Round C)
- **Idiomatic** — uses the protocol the intended way (no half-adoption — see
  protocol-gotchas #2)
- **Spark** — (Round C only) creativity / fit to a real use-case

### Why this is the demo, not just a feature
It exercises **AG-UI** (streamed critique), **A2UI** (the scorecard + leaderboard
surface), **MCP** (it can read the exercise docs), structured output (the rubric),
and the four AIPLA extensions (per-cohort budget gates the grading spend;
anonymous-group keeps it account-free; tenant attribution + artefact review apply
to submitted code). Detailed build in [`helper-agent-design.md`](helper-agent-design.md).

> **Integrity note:** the reference solution lives on `main`; don't ship the
> rubric's expected answers in the client. Grade server-side in the helper skill.

## What ships in the fork (asset checklist)

- [ ] Pinned tag off the template + `workshop-start` branch with 3 blanked spots
- [ ] `docs/exercises/{agui,a2ui,mcp}.md` (mirror the Round-B cards)
- [ ] Round-C skeleton skill + a worked example
- [ ] Helper-agent skill (RAG corpus + `submit_for_show_and_tell` + `rate_submission`)
- [ ] Anonymous-group join pre-configured (printable code for the slide)
- [ ] A verified `make dev-local` green path (drives [`pre-work.md`](pre-work.md))

## Open decisions

- **Fork name** — confirm `build-ai-uis-workshop-app` (or pick another).
- **Submission transport** — paste-in-chat (zero friction, no GitHub needed) vs
  branch/PR link (more realistic, needs GitHub accounts). Recommend **paste-in-chat
  as default**, link as optional.
- **Who authors the blanks** — needs someone with the fork checked out to cut the
  three exercises and confirm the success checks actually pass from `workshop-start`.
