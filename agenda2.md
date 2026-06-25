# Agenda v2 — activity-led redesign

> **Shift:** from "instructor presents for 3h" to **~⅔ student-led activity, ⅓ framing**.
> The slide deck does double duty: instructor running order *and* a self-paced
> **course guide** students browse on the website (each activity is a
> self-contained card: goal, steps, done-when, hints, solution).

## Design principles (the pedagogy, named)

The audience is education specialists, so the structure is intentional:

- **Think-pair-share / brain breaks** — break the protocol tour every ~6 min with a 2-min "turn to your neighbour" prompt. No passive stretch longer than that.
- **Active sense-making** — students explore a live demo and articulate what a protocol enables, rather than being told. (The *with vs without* contrast is shown once by the instructor in the tour — a per-group toggle is too heavy to set up.)
- **Faded worked examples (Parsons-style)** — exercises ship with the key code removed; students restore it (AI-coding allowed), rather than writing from a blank page.
- **Jigsaw** — each group masters one protocol, then teaches the others. Coverage without everyone doing everything.
- **Project-based + reflection** — groups plan a protocol app for *their own* context and reflect.
- **Peer presentation / gallery walk** — groups show what they made; social momentum, not instructor verdict.

## Format

- **Half-day, ~3h** (180 min incl. buffer). Modular — drop a round if the room is small or running late.
- **Tables of 3–5.** If the crowd is large, everything runs in groups; rotate/jigsaw between rounds.
- **AI coding encouraged** during build rounds. The **helper agent** answers FAQs and accepts show-and-tell submissions.
- **Instruction ≈ 50 min · activity ≈ 105 min · break ≈ 10 min.**

## Running order

| # | Min | Mode | What happens | Deck / card |
|---|---|---|---|---|
| 0 | 15 | Instructor + pair | Welcome · the chat wall · the protocol stack. **Brain break #1:** "where does text-only AI hit a wall in *your* work?" | `00-welcome`, `01-chat-wall` |
| 1 | 25 | Instructor + brain breaks | Guided tour of the 3 protocols — ~6 min demo each (AG-UI · A2UI · MCP Apps), each followed by a 2-min pair talk "how could you use this?". Real apps (AIPLA, GDE) flashed as "this ships." | `02-three-protocols` |
| 2 | 20 | Groups | **Round A — Explore a live skill.** Each table picks ONE running demo, pokes it, and lists 3 things the protocol enables that plain chat can't. (The instructor shows the with/without contrast in the tour — groups don't run two versions.) Share across tables. | `A1-explore-a-skill` |
| – | 10 | Break | — | — |
| 3 | 40 | Groups (jigsaw) + AI coding | **Round B — Reconstruct the skill.** Each group gets a skill with the key code removed and restores it (docs + helper agent + AI coding). One protocol per group → regroup so each new group has one "expert" per protocol; teach each other. | `B0-how-it-works`, `B1-agui`, `B2-a2ui`, `B3-mcp` |
| 4 | 25 | Groups + reflect | **Round C — Plan your own.** Sketch a protocol app for your context (lesson tool, intake form, dashboard…). Reflection prompts. AIPLA/GDE as worked examples. | `C1-plan-your-app` |
| 5 | 20 | Groups present | **Show & tell.** 2–3 min per group: what you reconstructed / planned. Submit via the helper agent; workspace pane updates live. | `show-and-tell` |
| 6 | 10 | Instructor + Q&A | **Wrap-up.** Synthesis · the 4 advanced patterns (AIPLA extensions) · where-next · Q&A. | `07-wrap-up`, `08-built-on-this`, `contact-mark` |

**Total:** 165 min + ~15 min buffer ≈ 3h.

## How the deck doubles as a course guide

Every activity is an **activity card** — a self-contained slide (or short set) a
student can follow alone on the website, not just an instructor cue:

```
ACTIVITY B2 · Render a form with A2UI            ⏱ 12 min · 👥 group of 3 · 🧠 jigsaw
🎯 Goal      One sentence — what you'll have working at the end.
1. Steps     Numbered, concrete, copy-pasteable.
2. …
✅ Done when  The observable success check ("the form renders in the workspace pane").
💡 Hints      Use the helper agent · protocol-gotchas.md · your AI coding tool.
🔓 Solution   Link to the completed file in the platform repo / a reveal slide.
```

This is the one genuinely new slide type to build (`activity-card`); the intro
and wrap decks reuse what we already have.

## What changes vs the current decks

- **Keep & trim:** `00-welcome`, `01-chat-wall`, the protocol explainers (fold blocks 2–4 into one tighter `02-three-protocols` tour), `08-built-on-this`, `contact-mark`, `about-mark`.
- **Replace:** the demo-heavy walkthrough blocks and the single "build your own (3 paths)" block become the **activity rounds A/B/C** above (group-led, faded-code, jigsaw).
- **Add:** the `activity-card` slide type + a card per activity, written to stand alone on the website.
- **Demo thread stays:** AG-UI/A2UI/MCP demo skills are now what groups *manipulate* in Rounds A & B, not what they watch.

---

## Original notes (source — preserved)

```
presentation of the three protocols 25mins or longer or intersperse with brain breaks - speak to partner about how they could possibly use
maybe rotate around tables
QnA
while they are coding
if large crowd do it in groups
they can use ai coding
code exercises - edit out solution then add it in
compare with and without protocols
breaks
Make a plan for the app to use - reflect on what
people present what they have done
10min wrap up
```
