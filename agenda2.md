# Agenda v2 — activity-led redesign

> **Shift:** from "instructor presents for 3h" to **~⅔ student-led activity, ⅓ framing**.
> The slide deck does double duty: instructor running order *and* a self-paced
> **course guide** students browse on the website (each activity is a
> self-contained card: goal, steps, done-when, hints, solution).

## Design principles (the pedagogy, named)

The audience is education specialists, so the structure is intentional:

- **Think-pair-share / brain breaks** — break the protocol tour every ~6 min with a 2-min "turn to your neighbour" prompt. No passive stretch longer than that.
- **Active sense-making** — students explore a live demo and articulate what a protocol enables, rather than being told.
- **Contrast cases (homespun vs protocol)** — for each protocol, groups see the hand-rolled way and its pain, then the protocol itself (mostly in a key-free dev playground), and name what it removed. A faded-code reconstruct (restore the blanked line, AI-coding allowed) is an optional advanced tier.
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
| 3 | 40 | Groups (jigsaw) | **Round B — What the protocols do.** Each group takes ONE protocol: see the homespun way, then play with the real thing in its dev playground (A2UI `/dev/a2ui`, MCP `/dev/mcp-apps/active`, AG-UI in DevTools). Regroup so each new group has one expert per protocol; teach back *what it does + what it replaces*. (Optional advanced: restore the blanked code on `workshop-start`.) | `B0-how-it-works`, `B1-agui`, `B2-a2ui`, `B3-mcp` |
| 4 | 25 | Groups + reflect | **Round C — Plan your own.** Sketch a protocol app for your context (lesson tool, intake form, dashboard…). Reflection prompts. AIPLA/GDE as worked examples. | `C1-plan-your-app` |
| 5 | 20 | Groups present | **Show & tell.** 2–3 min per group: what your protocol does + what you built/planned. Submit via the helper agent; workspace pane updates live. | `show-and-tell` |
| 6 | 10 | Instructor + Q&A | **Wrap-up.** Synthesis · the 4 advanced patterns (AIPLA extensions) · where-next · Q&A. | `07-wrap-up`, `08-built-on-this`, `contact-mark` |

**Total:** 165 min + ~15 min buffer ≈ 3h.

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

## What changes vs the current decks

- **Keep & trim:** `00-welcome`, `01-chat-wall`, the protocol explainers (fold blocks 2–4 into one tighter `02-three-protocols` tour), `08-built-on-this`, `contact-mark`, `about-mark`.
- **Replace:** the demo-heavy walkthrough blocks and the single "build your own (3 paths)" block become the **activity rounds A/B/C** above (group-led, homespun-vs-protocol + dev playgrounds, jigsaw).
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
