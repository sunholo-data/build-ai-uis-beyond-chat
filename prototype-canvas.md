# Prototype canvas — plan your own protocol app

> **Round C** (~25 min, in groups). Don't start from a blank page.
> Fill each field below with one line. Copy this template, one per app idea.
> The point isn't a finished spec — it's a shared sketch your group can argue about.

---

## The canvas

**The task / problem** — a real thing from *your* work, not a toy demo.

> _______________________________________________

**Who it's for** — the person who hits "go" and the person who sees the result.

> _______________________________________________

**Which surface fits** — chat bubble · workspace pane · sandboxed iframe widget. (Pick one. Why that one?)

> _______________________________________________

**Which protocol(s)** — AG-UI (always — it's the transport) · A2UI (declarative UI) · MCP Apps (interactive widget) · MCP tools / A2A.

> _______________________________________________

**The ONE skill + ONE tool it needs** — resist adding a second of either.

> Skill: ______________________   Tool: ______________________

**Inputs / data it reads** — what does the agent need in front of it?

> _______________________________________________

**One risk** — auth, cost, or safety. Just the biggest one.

> _______________________________________________

**Success in one sentence** — "you'll know it works when…"

> _______________________________________________

---

## Worked example — lesson exit-ticket

A quick comprehension check a teacher fires at the end of class.

| Field | Filled in |
|---|---|
| **Task / problem** | After a lesson, a teacher wants a 30-second read on who got it — without printing slips or opening a separate quiz tool. |
| **Who it's for** | The teacher (asks for the check, reads the result); the students (answer on their own devices). |
| **Surface** | **Workspace pane** — the form needs room and stays put while the class answers; a chat bubble is too cramped. |
| **Protocol(s)** | **AG-UI** (transport, always) + **A2UI** to render the form declaratively. No custom React per question. |
| **Skill + tool** | Skill: `exit-ticket`. Tool: `save_response` (one tool, writes each answer). |
| **Inputs / data** | The lesson topic + the one question the teacher types; student answers as they arrive. |
| **One risk** | **Safety / data** — student responses are personal. Anonymise on the way in; show the teacher a tally, never named answers. |
| **Success** | "You'll know it works when the teacher types one question, students answer on their phones, and a live tally updates in the workspace pane." |

The teacher never touches a quiz builder. They ask; A2UI renders the form to the workspace; `save_response` collects; surface state rides back so the agent can report the tally.

---

## Reflect (2 lines, as a group)

> What got **easier** by leaning on the protocols instead of building this as a custom UI?

> What's still **awkward** — where did the protocol fight you, or where would you reach for something else?
