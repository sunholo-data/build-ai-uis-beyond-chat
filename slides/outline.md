# Slide outline — Build AI UIs Beyond Chat

The talk is a sequence of self-contained decks, played in order through
[`presenter.html`](presenter.html). This file is the **source of truth** for the
deck order and per-deck time budgets — keep the `PLAYLIST` `minutes` in
`presenter.html` in sync with the table below.

Block content is drawn from [`../agenda.md`](../agenda.md) (the instructor's
running order). Block budgets sum to **180 min** (3h); the book-end decks
(Welcome, About Mark, Contact) add a few minutes of front/back matter.

| # | Deck | Agenda block | Min | Status |
|---|---|---|---|---|
| 1 | [`00-welcome.html`](00-welcome.html) | (cold open) | 2 | 🟡 skeleton |
| 2 | [`about-mark/about-mark.html`](about-mark/about-mark.html) | (standard intro) | 2 | ✅ reused |
| 3 | [`01-chat-wall.html`](01-chat-wall.html) | Block 0 — The Chat Wall Problem | 10 | 🟡 skeleton |
| 4 | [`02-quickstart.html`](02-quickstart.html) | Block 1 — Quick-start verification | 10 | 🟡 skeleton |
| 5 | [`03-adk-agui.html`](03-adk-agui.html) | Block 2 — ADK + AG-UI | 30 | 🟡 skeleton |
| 6 | [`04-a2ui.html`](04-a2ui.html) | Block 3 — A2UI + multi-surface | 35 | 🟡 skeleton |
| 7 | [`05-mcp-apps.html`](05-mcp-apps.html) | Block 4 — MCP Apps + sandbox | 30 | 🟡 skeleton |
| 8 | [`06-build-skill.html`](06-build-skill.html) | Block 5 — Build your own skill ⭐ | 55 | 🟡 skeleton |
| 9 | [`07-a2a-close.html`](07-a2a-close.html) | Block 6 — A2A discovery + close | 10 | 🟡 skeleton |
| 10 | [`08-built-on-this.html`](08-built-on-this.html) | (showcase — AIPLA + GDE) | 4 | 🟢 drafted |
| 11 | [`contact-mark/contact-mark.html`](contact-mark/contact-mark.html) | (standard close) | 1 | ✅ reused |

**Total budget:** 189 min (180 of agenda blocks + 9 of book-ends/showcase).

## Demo thread — two real apps, demoed throughout

Two production forks of the platform run as the real-world demo thread: each
protocol block shows the **clean demo skill first** (minimal, LOCAL_MODE), then
the **same protocol shipping in a real app**.

- **AIPLA** — *AI in Physics Learning & Assessment* ([cphu-aipla-app](https://github.com/sunholo-data/cphu-aipla-app)) ·
  live: `https://aipla-v01-frontend-wgwhd7mspa-lz.a.run.app/teacher/classes/0b99f4e04792`
- **GDE AP Agent** — *invoice / accounts-payable* ([gde-ap-agent](https://github.com/sunholo-data/gde-ap-agent)) ·
  live: `https://gde-ap-agent-blqtqfexwa-ew.a.run.app/`

| Block | Protocol | Clean demo skill | Real-app demo |
|---|---|---|---|
| 00-welcome | (intro) | — | both apps — "demoed throughout" teaser |
| 03 / Block 2 | ADK + AG-UI | `demo-researcher` | GDE live pipeline visualizer · AIPLA tutor stream |
| 04 / Block 3 | A2UI multi-surface | `demo-form-builder` → `demo-workspace` | GDE `InvoiceHeroCard` · AIPLA workspace surface |
| 05 / Block 4 | MCP Apps | `demo-map-explorer` | GDE Analytics Dashboard + Vendor Knowledge Graph (bidirectional) · AIPLA `led-planck` sim |
| 06 / Block 5 | Build a skill | skeleton-skill | both are forks: GDE = 5 `SKILL.md` (~80 lines); AIPLA = `manage-class` · `problem-set-hints` · `analytics-chat` |
| 07 / Block 6 | A2A + prod patterns | `curl /.well-known/agent.json` | AIPLA → the 4 template-extensions; GDE marketplace submission |
| 08-built-on-this | (showcase) | — | full deep-dive on both |

> **Practical note for live demoing:** the demo skills run locally in
> `LOCAL_MODE=1`; the two real apps are deployed on Cloud Run (the AIPLA link is
> a teacher view that may require sign-in). Decide per block whether to click
> through live or show a capture. The apps **augment** — they don't replace —
> the clean demo skills, which stay the primary "watch the wire" vehicle.

## Status legend

- ✅ **reused** — standard book-end deck copied from the `sunholo-data/presentations`
  repo; edit only if this talk needs a variant.
- 🟡 **skeleton** — title + agenda-derived beats only. Expand each into real
  slides (the `.note` line on the last slide of each block points back here).

## Build order suggestion

The two highest-leverage decks to flesh out first are the two longest blocks:
**`03-adk-agui`** (the "watch the wire" core) and **`06-build-skill`** (the
make-or-break hands-on). The shorter framing decks (chat-wall, a2a-close) can
stay lean.
