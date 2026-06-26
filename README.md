# Build AI UIs Beyond Chat — Workshop Materials

> The new agent UI protocols let AI generate real interfaces, not just text. Build with all three in one session: **MCP Apps**, **A2UI**, **AG-UI**.

This repo contains the **materials** for the workshop. The **code** lives separately at [sunholo-data/ai-protocol-platform](https://github.com/sunholo-data/ai-protocol-platform).

> 📝 This repo is the **source of truth** for the workshop materials — edit them here. (They were originally generated from the platform repo's `docs/workshop/`, but that one-way sync is retired.)

## Two repos, one workshop

| Repo | What it contains |
|---|---|
| [sunholo-data/ai-protocol-platform](https://github.com/sunholo-data/ai-protocol-platform) | The platform code — clone this, run `make dev-local`, build skills on it. |
| **This repo** | The workshop materials — agenda, code tour, skeleton skills, helper-agent design, slides. |

You'll clone both for the workshop. The platform repo is the canonical source-of-truth code; this repo is the curriculum that wraps around it.

## Workshop at a glance

- **Format:** In-person — two 75-min blocks split by a 30-min coffee break (Web Summer Camp, Fri 3 Jul 2026, 10:00–13:00)
- **Audience:** Developers with some Python + React familiarity
- **Outcome:** Hands-on with all three protocols in dev playgrounds · a forked platform you own · a plan for your own protocol app
- **Shape:** ~⅔ student-led activity, ⅓ framing — group work at tables, AI coding encouraged

| Block | What you'll do |
|---|---|
| **Block 1 · framing + explore** (10:00–11:15) | The chat-wall problem · a guided tour of all three protocols · **Round A** — explore a live skill at your table · pick up your Round-B protocol to mull over coffee |
| ☕ Coffee break (11:15–11:45) | — |
| **Block 2 · build + share** (11:45–13:00) | **Round B** — jigsaw: master one protocol in a dev playground, then teach it back · **Round C** — plan your own protocol app · show & tell · wrap-up |

Full running order + pedagogy: [`agenda.md`](agenda.md).

## Sessions

- **First:** WebSummerCamp Croatia 2026 — **Fri 3 Jul 2026, 10:00–13:00** ([websummercamp.com](https://websummercamp.com/2026/news/super-early-sold-out-get-your-early-bird-now))
- Tue 18 Aug 2026
- Tue 17 Nov 2026
- Tue 16 Feb 2027
- + more TBA

## Materials

| Doc | Status |
|---|---|
| [`agenda.md`](agenda.md) | ✅ The **activity-led** running order + pedagogy (two-block schedule) |
| [`facilitator-guide.md`](facilitator-guide.md) | ✅ Instructor playbook — how to run each activity, jigsaw mechanics, triage |
| [`slides/`](slides/) | ✅ The deck — open [`slides/presenter.html`](slides/presenter.html); order/timer in [`slides/outline.md`](slides/outline.md) |
| [`pre-work.md`](pre-work.md) | ✅ 24–48h before-arrival setup + verify checklist |
| [`workshop-fork.md`](workshop-fork.md) | ✅ The runnable fork: exercises, start↔solution, solution-rating design |
| [`helper-agent-design.md`](helper-agent-design.md) | ✅ The meta-demo agent — RAG FAQ + show-and-tell + live solution rating |
| [`prototype-canvas.md`](prototype-canvas.md) | ✅ Round C worksheet — plan your own protocol app |
| [`cheat-sheet.md`](cheat-sheet.md) | ✅ 1-page protocol quick reference for the build rounds |
| [`code-tour.md`](code-tour.md) | ✅ 7-file ~1,900 LOC reading map (take-home) |
| [`protocol-gotchas.md`](protocol-gotchas.md) | ✅ 13 protocol bear traps (AG-UI, A2UI, MCP Apps, ADK) |
| skeleton skill | ⬜ Lives in the workshop fork (Round C starter) — see [`workshop-fork.md`](workshop-fork.md) |

## Ask the helper agent anything

The workshop's **helper agent** runs on the same platform you're learning. It's
preloaded with this repo's docs PLUS the platform's full design corpus (every
shipped sprint design doc, every integration howto, every gotcha). Questions
it can answer with real ground truth:

- "How does AG-UI's `state` channel actually behave on the wire?"
- "What are the seven access gates on `/api/sessions/{id}/iframe-context`?"
- "Why does the A2UI v0.9 wrapper silently drop my spec?"
- "Show me where multi-provider routing lives in the agent factory."

Answers come from RAG over real `docs/design/v6.X.Y/implemented/` content,
not the model's prior knowledge. **The helper agent is itself the meta-demo:**
it uses AG-UI for streaming, A2UI for the workspace, MCP for the docs index,
and demonstrates the four advanced AIPLA patterns (anonymous-group auth,
per-cohort budget, artefact review, tenant-id span attribution).

## Pre-work (short version)

Before the workshop:

1. Install Node 20+ and Python 3.11+
2. Clone the platform repo: `git clone https://github.com/sunholo-data/ai-protocol-platform.git`
3. Run `make dev-local` — confirm the chat UI works at `http://localhost:3456`
4. Bring a laptop with a GitHub account ready

You do NOT need: Docker, GCP credentials, Firebase. The platform runs in `LOCAL_MODE=1` with in-memory stubs.

## Take-home

Every attendee leaves with:
- Hands-on experience driving all three protocols in their dev playgrounds
- A fork of the public template they own, running locally
- A one-page plan for a protocol app in their own context (Round C)
- A clear path to the four advanced patterns (sprints 2.11–2.14)
- A 5-minute read in the platform repo at [`docs/ops/deployment-models.md`](https://github.com/sunholo-data/ai-protocol-platform/blob/main/docs/ops/deployment-models.md) for day-one of operating their fork — picks single-service Cloud Run sidecar vs paired sidecar+standalone *before* first push, so the unused `cloudbuild.yaml` doesn't quietly fail every CI build for months

## License

Apache 2.0. Copyright 2026 Holosun ApS. Same as the platform repo.
