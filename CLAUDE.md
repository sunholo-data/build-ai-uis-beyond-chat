# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

`sunholo-data/build-ai-uis-beyond-chat` is the **public curriculum** for the "Build AI UIs Beyond Chat" workshop — a half-day, hands-on session on the agent-UI protocol stack (**MCP Apps**, **A2UI**, **AG-UI**, with A2A + ADK underneath).

It holds **materials, not code**: agenda, facilitator guide, code-tour, protocol-gotchas, pre-work, helper-agent design, the workshop-fork guide, and slides. The runnable code attendees clone and build on lives in a *different* repo (see lineage below). There is no build, no test suite, no app to run here — every file is Markdown, except the self-contained HTML slide decks under `slides/`.

## The three-repo lineage (read this first)

This repo is the public, downstream tip of a three-stage chain. Knowing where a fact's source-of-truth lives is the single most important thing when working here.

| Repo | Visibility | Role | Local path |
|---|---|---|---|
| **`Aitana-Labs/platform`** | private | **Source of truth.** The internal platform: ~13k backend LOC + ~9k frontend LOC, all design docs, customer/NDA bits, the Agents CLI, and the workshop source under `docs/workshop/`. | `/Users/mark/dev/aitana-labs/platform` |
| **`sunholo-data/ai-protocol-platform`** | public | The **sanitized code template** derived from the platform (customer content, NDA refs, and Agents CLI stripped by `scripts/sanitize-for-template.sh` + `refresh-public-template.sh`). This is the codebase attendees clone and run with `LOCAL_MODE=1`. | not cloned locally |
| **`sunholo-data/build-ai-uis-beyond-chat`** (this repo) | public | The **workshop curriculum** that wraps around the public template. | this directory |

Two generators live in the platform repo, both historically force-push (fresh `git init` → single squashed commit → `git push --force`):
- `scripts/refresh-public-template.sh` → publishes the code template (`ai-protocol-platform`). **Still active.**
- `scripts/refresh-workshop-materials.sh` → previously published **this** repo from `platform:docs/workshop/`. **Retired** — must not be run (see next section).

This repo's current single-commit history (`Refresh workshop materials from sunholo-data/ai-protocol-platform <sha>`, where `<sha>` was the platform HEAD that generated it) is the last generated snapshot. From here on, history accumulates normally as you commit edits.

## Source of truth: this repo

This repo is now the **authoritative home** of the workshop materials. Edit the Markdown files here directly and commit them here. (These files were originally generated from `platform:docs/workshop/` and force-pushed down; that one-way sync is **retired**.)

> ⚠️ Do NOT run `platform/scripts/refresh-workshop-materials.sh`. It does a fresh `git init` + `git push --force` onto this repo's `main`, which would **wipe this repo's history and overwrite every file** — including this CLAUDE.md and any hand-authored materials. The script has been deprecated/guarded in the platform repo. (`refresh-public-template.sh`, which publishes the *code* template `ai-protocol-platform`, is unrelated and stays active.)

Consequence of the flip: `platform:docs/workshop/` is now a **stale copy** — don't treat it as canonical, and don't sync from it. If the workshop helper agent's RAG corpus needs these materials, the sync should run *from this repo back up to the platform* (the reverse of the old direction); that reverse script does not exist yet.

## Cross-repo link convention

When editing the materials (wherever they live), Markdown links follow a strict rule the refresh script's `sed` rewrites enforce:

- **Intra-workshop links stay relative**: `[code-tour.md](code-tour.md)`, `[agenda.md](agenda.md)`.
- **Links into platform code/docs are absolute GitHub URLs** to the *public template*: `https://github.com/sunholo-data/ai-protocol-platform/blob/main/...`. Write these directly — there is no longer a sync script to rewrite relative escapes for you, and the existing files already use this absolute form. Never link to the private `Aitana-Labs/platform` repo from these public materials.

When writing file/line references in *responses* (not in the materials), use the VSCode markdown-link form per the harness rules.

## What the materials cover (orientation)

- **`README.md`** — the public landing page (hand-maintained here; it originated from `_workshop-readme.md` upstream). Two-repo setup, schedule, pre-work, take-home.
- **`agenda.md`** — the instructor's running order: the **activity-led**, two-block schedule (2 × 75 min split by a hard 30-min coffee break; confirmed Web Summer Camp, Fri 3 Jul 2026). Block 1 = framing + Round A explore + Round-B handoff; Block 2 = Round B build/teach-back + Round C plan + show-and-tell + wrap. Pedagogy (jigsaw, brain breaks, contrast cases) up top.
- **`facilitator-guide.md`** — the instructor playbook (the *how*): pre-flight, running the timer, per-block run notes, the jigsaw in detail, setup triage, and the cut order. Pairs with `agenda.md` (the *what*).
- **`code-tour.md`** — a 7-file, ~1,900 LOC reading map into the *platform* code (the protocol-interaction code is small; most of the platform is production scaffolding the tour tells you to skip).
- **`protocol-gotchas.md`** — 13 field-tested bear traps in AG-UI / A2UI / MCP Apps / ADK, each mapped to the workshop block where it bites. Source of truth for these is `platform:docs/talks/ai-ui-protocol-stack.md` (the living verification log).
- **`pre-work.md`**, **`workshop-fork.md`**, **`helper-agent-design.md`**, **`prototype-canvas.md`**, **`cheat-sheet.md`** — before-arrival setup; the runnable fork (blanked exercises + dev playgrounds); the meta-demo FAQ/rating agent; the Round C worksheet; and the 1-page protocol quick reference.
- **`slides/`** — the HTML slide decks (see below).
- Still TBD: the **skeleton skill** (Round C starter) lives in the workshop fork, not here — see `workshop-fork.md`.

## Slides (`slides/`)

Self-contained HTML decks (no build step), one per agenda block plus reused
book-end decks. Built on the **`presentation-slides`** skill vendored into this
repo at `.claude/skills/presentation-slides/` (design system, slide-type
patterns, boilerplate, and a stage-safe linter). The skill originates from the
`sunholo-data/presentations` repo.

- Play the talk via `slides/presenter.html`; deck order + per-deck minutes are
  defined in `slides/outline.md` (the source of truth — mirror it in the
  presenter's `PLAYLIST`).
- **Hard rule when editing decks:** size slide content with `cqi`/`cqb`/`%`/flex,
  never `vh`/`vw` (the 16:9 stage is letterboxed; viewport units overflow it).
  The only allowed viewport unit is the single `#app{…;height:100vh}` shell line.
  Verify with `node .claude/skills/presentation-slides/scripts/lint-slides.mjs slides/*.html`.
- The deck is **activity-led** (see `agenda.md`): instructor intro
  (`00-welcome`, `01-chat-wall`, `02-three-protocols`) → student activity rounds
  (`A1`, `B0`+`B1`/`B2`/`B3` jigsaw, `C1`) → `show-and-tell` → `07-wrap-up` →
  `08-built-on-this` showcase, wrapped in reused `about-mark`/`contact-mark`.
  The `B0` handoff closes Block 1 (pre-break); the `B1`–`B3` build opens Block 2.
- Activity decks use the `activity-card` layout (`.act`: goal · steps · done-when
  · hints · solution) so they double as a **self-paced course guide** on the
  website. See `slides/README.md` for layout, asset-path rules, the demo thread,
  and the (not-yet-enabled) GitHub Pages publishing note.

## Editing tone

These are public-facing teaching materials with a distinct voice: direct, field-notes-not-theory, "watch the wire" concreteness, honest about what's TBD. Match it. The protocol-gotchas and code-tour earn credibility by being specific (exact function names, line ranges, commits) rather than abstract — preserve that specificity when editing.
