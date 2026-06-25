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

## The Round B exercises (jigsaw)

Each protocol exercise follows the same shape — **homespun (the pain) → the protocol
(play with it) → what the protocol removed** — so groups learn what the protocol
*does*, not just how to patch a line. Most of it is **key-free** (dev playgrounds).
Full text in `docs/exercises/{agui,a2ui,mcp}.md` (identical on both branches).

### B1 · AG-UI — a typed event stream
- **Homespun:** POST + `await` the whole reply → no streaming, tool calls invisible,
  your own ad-hoc wire shape.
- **Protocol:** `agent.subscribe({…})` maps 16 typed events → React state. Read the
  live SSE: DevTools → Network → `stream` *(needs a reply)*.
- **Advanced reconstruct (`workshop-start`):** restore `onMessagesChanged` in
  `frontend/src/hooks/useSkillAgent.ts`. ✅ `vitest useSkillAgent.test.tsx`.

### B2 · A2UI — UI as JSON (key-free) ⭐
- **Homespun:** a hardcoded React `<ContactForm/>` → a new form means new code + a
  redeploy.
- **Protocol:** the form is a JSON component tree. Play at **`/dev/a2ui`** — edit
  `PATTERN1_SEED_MESSAGES`, hot-reload, watch it render. No agent, no key.
- **Advanced reconstruct (`workshop-start`):** restore `default_surface` on
  `demo-workspace` in `backend/db/local_fixture.py`. ✅ `pytest test_demo_workspace_surface.py`.

### B3 · MCP Apps — sandboxed widgets + two channels (key-free) ⭐
- **Homespun:** a raw `<iframe>` → it can read your cookies, no back-channel,
  bespoke postMessage glue.
- **Protocol:** UI-by-reference + sandboxed origin + `ui/message` &
  `ui/update-model-context`. Play at **`/dev/mcp-apps/active`** — fire both channels
  through the real bridge (on-page log). No iframe, no key.
- **Advanced reconstruct (`workshop-start`):** restore the `ui/update-model-context`
  POST in `frontend/src/components/protocols/MCPAppToolCallRouter.tsx`.
  ✅ `vitest MCPAppToolCallRouter.iframeContext.test.tsx`.

> Solutions live on `main`; reveal any reconstruct with
> `git diff workshop-start main -- <file>`. All three reconstructs are built and
> test-verified (fail on `workshop-start`, pass on `main`).

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

- [x] Repo created + seeded + `workshop-start` branch with 3 blanked spots
- [x] `docs/exercises/{README,agui,a2ui,mcp}.md` (homespun-vs-protocol + playgrounds)
- [x] Verified `make dev-local` green path (drives [`pre-work.md`](pre-work.md))
- [x] Codespaces devcontainer (one-click, any OS — see [`pre-work.md`](pre-work.md))
- [ ] Round-C skeleton skill + a worked example
- [ ] Helper-agent skill (RAG corpus + `submit_for_show_and_tell` + `rate_submission`)
- [ ] Anonymous-group join pre-configured (printable code for the slide)

## Open decisions

- ~~**Fork name**~~ — done: `sunholo-data/build-ai-uis-workshop-app` (public).
- ~~**Who authors the blanks**~~ — done: all three cut + test-verified.
- **Submission transport** (for the rating feature) — paste-in-chat (zero friction,
  no GitHub needed) vs branch/PR link (more realistic, needs GitHub accounts).
  Recommend **paste-in-chat as default**, link as optional.
