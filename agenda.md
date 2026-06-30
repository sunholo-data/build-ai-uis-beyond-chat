# Workshop Agenda — Build AI UIs Beyond Chat

> The new agent UI protocols let AI generate real interfaces, not just text — build with all three in one session: **MCP Apps**, **A2UI**, **AG-UI**.

**Audience:** developers with some Python + React familiarity.
**Format:** ~⅔ student-led activity, ⅓ framing — two 75-min blocks split by a 30-min coffee break (see [Format](#format)).
**Outcome:** hands-on with all three protocols in dev playgrounds · a forked platform you own · a plan for your own protocol app.

The slide deck does double duty: instructor running order *and* a self-paced
**course guide** students browse on the website (each activity is a
self-contained card: goal, steps, done-when, hints, solution).

## Design principles (the pedagogy, named)

The audience is education specialists, so the structure is intentional:

- **Think-pair-share / brain breaks** — break the protocol tour every ~6 min with a 2-min "turn to your neighbour" prompt. No passive stretch longer than that.
- **Active sense-making** — students explore a live demo and articulate what a protocol enables, rather than being told.
- **Contrast cases (homespun vs protocol)** — for each protocol, groups see the hand-rolled way and its pain, then the protocol itself (mostly in a key-free dev playground), and name what it removed. A faded-code reconstruct (restore the blanked line, AI-coding allowed) is an optional advanced tier.
- **Jigsaw** — each group masters one protocol, then teaches the others. Coverage without everyone doing everything.
- **Project-based + reflection** — groups plan a protocol app for *their own* context and reflect.
- **Peer presentation / gallery walk** — groups show what they made; social momentum, not instructor verdict.

## Format

**Confirmed slot — Web Summer Camp, Fri 3 Jul 2026.** Two 75-min blocks with a hard
30-min coffee break between, not one continuous 3h. This drives the whole structure:

| | Wall clock | What |
|---|---|---|
| Coffee hangout | 09:00–10:00 | Pre-session — **get everyone's Codespaces + Gemini key working before the clock starts** (the pre-work setup pain dies here, not in Block 1). |
| **Block 1** | 10:00–11:15 (75) | Framing · explore · Round-B handoff. |
| ☕ Hard break | 11:15–11:45 (30) | Room empties — Block 1 must end self-contained. |
| **Block 2** | 11:45–13:00 (75) | Build · plan · share · wrap. |
| Lunch | 13:00–14:15 | (Hotel Ambasador.) |

- **150 min on the clock** (2 × 75), break off-clock. Budgeted to ~145 to leave slack.
- The **make-or-break Round B jigsaw must not straddle the break**: its *handoff*
  (assign protocol + read the exercise) happens at the end of Block 1 so coffee chat
  primes it; the *build + teach-back* runs contiguous at the top of Block 2.
- **Tables of 3–5.** If the crowd is large, everything runs in groups; rotate/jigsaw between rounds.
- **AI coding encouraged** during build rounds. The **helper agent** answers FAQs and accepts show-and-tell submissions.
- **Instruction ≈ 48 min · activity ≈ 95 min · hard break 30 min (off-clock).**

## Running order

### Block 1 — Framing, Explore, Round-B handoff · 10:00–11:15 (75)

| # | Min | Mode | What happens | Deck / card |
|---|---|---|---|---|
| 0 | 15 | Instructor + pair | Welcome · the chat wall · the protocol stack. **Brain break #1:** "where does text-only AI hit a wall in *your* work?" | `00-welcome`, `about-mark`, `01-chat-wall` |
| 1 | 25 | Instructor + brain breaks | Guided tour of the 3 protocols — walk the **on-the-wire explainer decks**, ~6 min each (AG-UI · A2UI · MCP Apps), each followed by a 2-min pair talk "how could you use this?". Real apps (AIPLA, GDE) flashed as "this ships." | `02-three-protocols` → `wire-overview` / `wire-agui` / `wire-a2ui` / `wire-mcp` / `wire-sources` |
| 2 | 20 | Groups | **Round A — Explore a live skill.** Each table picks ONE running demo, pokes it, and lists 3 things the protocol enables that plain chat can't. (The instructor shows the with/without contrast in the tour — groups don't run two versions.) Share across tables. | `A1-explore-a-skill` |
| 3a | 10 | Instructor → groups | **Round B handoff.** Explain the jigsaw, **assign each group its ONE protocol**, and have them open & read their exercise. Nothing to build yet — they carry the question into coffee. | `B0-how-it-works` |

*Block 1 on-clock ≈ 70 min; the spare ~5 absorbs intro/AV/latecomer overrun.*

### ☕ Hard break · 11:15–11:45 (30, off-clock)

Room empties for coffee. Groups already know their protocol — coffee chat is "what's your protocol, what's it replace?" **Pause the presenter clock** so the delta stays honest.

### Block 2 — Build, Plan, Share · 11:45–13:00 (75)

| # | Min | Mode | What happens | Deck / card |
|---|---|---|---|---|
| 3b | 35 | Groups (jigsaw) | **Round B — build + teach-back.** In your protocol's expert group: see the homespun way, then play with the real thing in its dev playground (A2UI `/dev/a2ui`, MCP `/dev/mcp-apps/active`, AG-UI in DevTools). Regroup so each new group has one expert per protocol; teach back *what it does + what it replaces*. (Optional advanced: restore the blanked code on `workshop-start`.) | `B1-agui`, `B2-a2ui`, `B3-mcp` |
| 4 | 15 | Groups + reflect | **Round C — Plan your own.** Sketch a protocol app for your context (lesson tool, intake form, dashboard…). Reflection prompts. AIPLA/GDE as worked examples. | `C1-plan-your-app` |
| 5 | 15 | Groups present | **Show & tell.** 2–3 min per group: what your protocol does + what you built/planned. Submit via the helper agent; workspace pane updates live. | `show-and-tell` |
| 6 | 10 | Instructor + Q&A | **Wrap-up.** Synthesis · the 4 advanced patterns (AIPLA extensions) · where-next · Q&A. | `07-wrap-up`, `08-built-on-this`, `contact-mark` |

**Total:** ~145 min on the clock across the two 75-min blocks (~5 min slack), + 30-min hard break off-clock.

## How the deck doubles as a course guide

Every activity is an **activity card** — a self-contained slide (or short set) a
student can follow alone on the website, not just an instructor cue:

```
ROUND B · A2UI — what it does               ⏱ 20 min · 👥 expert group · 🧠 contrast
🎯 Goal      See A2UI for what it is — UI as JSON the agent emits — vs hardcoding React.
1. Homespun  a hardcoded React <ContactForm/> → new form = new code + redeploy
2. Protocol  the same form as a JSON component tree
3. Play      /dev/a2ui — edit the JSON, hot-reload, watch it render (no key)
✅ Teach-back A2UI = declarative UI as data; one renderer draws anything
💡 Hints      docs/exercises/a2ui.md · the helper agent · your AI coding tool
🔓 Advanced   restore default_surface on workshop-start → pytest passes
```

This is the one genuinely new slide type (`activity-card`); the intro and wrap
decks reuse what we already have.

## Companion docs

- [facilitator-guide.md](facilitator-guide.md) — the instructor playbook (how to run each round, jigsaw mechanics, cut order).
- [slides/outline.md](slides/outline.md) — deck order + per-deck minute budgets (the presenter timer's source of truth).
- [workshop-fork.md](workshop-fork.md) — the runnable fork: the three blanked exercises + the dev playgrounds.
- [pre-work.md](pre-work.md) — what attendees do before arriving.
