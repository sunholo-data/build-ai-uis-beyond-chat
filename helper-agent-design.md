# Helper agent — design

The workshop's helper agent runs **on the same platform attendees are learning**,
so it is the meta-demo: it answers questions from the real docs corpus, runs
show-and-tell submissions, and **rates uploaded solutions live** — using every
protocol in the stack to do it.

## Two jobs

1. **Live FAQ** during the build rounds — answers from RAG over the workshop's own
   docs (so instructors can circulate instead of fielding the same question 20×).
2. **Submission + rating** — attendees upload a reconstruct or a prototype; the
   agent grades it against a rubric and renders a scorecard. See
   [`workshop-fork.md`](workshop-fork.md) → "Upload solutions" for the flow.

## It IS the demo

Every protocol the workshop teaches is load-bearing in the helper agent itself —
point this out in the wrap-up:

| Protocol | How the helper agent uses it |
|---|---|
| **AG-UI** | streams answers + streams the grading critique as it reasons |
| **A2UI** | renders the **scorecard** and the **live leaderboard** to the workspace surface |
| **MCP** | reads the exercise docs / corpus as an MCP resource |
| **A2A** | discoverable at `/.well-known/agent.json` like any skill |
| AIPLA ext. | per-cohort **budget** gates grading spend · **anonymous-group** join (no accounts) · **artefact review** + **tenant attribution** apply to submitted code |

## Tools

- `ask_docs(query)` — RAG over the corpus (below). Returns grounded answers with
  source links, **not** model priors.
- `submit_for_show_and_tell(group, payload)` — adds a submission to the live
  gallery in the workspace surface.
- `rate_submission(track, payload)` — the new asset; see below.
- `reveal_solution(track)` — gated peek at the reference (for a stuck group),
  equivalent to `git diff workshop-start main`.

## Rating engine (`rate_submission`)

**LLM-as-judge**, server-side in the helper skill (never ship expected answers to
the client). Input: the track + a pasted diff/snippet (default) or a branch link.

- **Round B (reconstruct)** — graded against the **reference solution** + the
  objective success check. Mostly deterministic.
- **Round C (prototype)** — graded against a rubric only (no single right answer).

**Rubric — 0–3 per dimension:**
- **Works** — success check passes (objective for Round B)
- **Correct** — matches reference behaviour / is technically sound
- **Idiomatic** — uses the protocol as intended (no half-adoption, gotchas #2)
- **Spark** — (Round C) creativity + fit to a real use-case

**Output:** a structured object (per-dimension score + one concrete next step) →
rendered as an **A2UI scorecard** in the workspace, with the critique **streamed**
over AG-UI. Submissions roll up into a workspace **leaderboard/gallery**.

> Keep it encouraging, not gatekeeping — the score exists to give fast, specific
> feedback to 8 groups at once, not to rank people. Default tone: "here's what
> works + the one thing to try next."

## Corpus (what RAG is preloaded with)

- This repo: `agenda.md`, `code-tour.md`, `protocol-gotchas.md`, the slides,
  `workshop-fork.md`, `cheat-sheet.md`.
- The fork's `docs/exercises/*` (so it can coach on the reconstructs).
- The platform's shipped design docs (`docs/design/v6.X.Y/implemented/`) +
  integration howtos — the deep ground-truth for "how does X actually work."

## Config sketch

- A `SkillConfig` in the fork: `instructions` (the coaching persona + rubric),
  `model` (a fast model — grading is high-volume), the four tools above, and
  `tool_configs.a2ui.surface_id = "workspace"` for the scorecard/leaderboard.
- **Per-cohort budget** (AIPLA ext.) so a runaway grading loop can't blow the day's
  spend — surfaces a typed `RUN_ERROR{BUDGET_EXCEEDED}` if hit.
- **Anonymous-group join** so attendees connect with a printed code, no signup.

## Build notes / dependencies

- Biggest single build — treat as its own sprint after the fork exercises exist
  (the rater needs the reference solutions on `main` to grade against).
- Reuses platform primitives: the A2UI toolset, the MCP docs index, LLM-as-judge
  (an eval pattern the platform already supports).
- **Open:** submission transport (paste-in-chat vs branch link — recommend
  paste-in-chat default); whether the leaderboard is per-table or whole-room.
