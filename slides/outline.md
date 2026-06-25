# Slide outline — Build AI UIs Beyond Chat (activity-led)

The talk is a sequence of self-contained decks played in order through
[`presenter.html`](presenter.html). Intro + wrap are instructor decks; the middle
is **student-led activity cards** that double as a self-paced course guide on the
website. This file is the **source of truth** for deck order and per-deck minutes
— keep the `PLAYLIST` in `presenter.html` in sync.

Full pedagogy + running order: [`../agenda2.md`](../agenda2.md).

| # | Deck | Phase | Min | Status |
|---|---|---|---|---|
| 1 | [`00-welcome.html`](00-welcome.html) | Intro — framing | 5 | 🟡 draft |
| 2 | [`about-mark/about-mark.html`](about-mark/about-mark.html) | Intro — standard | 2 | ✅ reused |
| 3 | [`01-chat-wall.html`](01-chat-wall.html) | Intro — the problem + brain break | 8 | 🟡 draft |
| 4 | [`02-three-protocols.html`](02-three-protocols.html) | Guided tour (instructor + brain breaks) | 25 | 🟡 draft |
| 5 | [`A1-explore-a-skill.html`](A1-explore-a-skill.html) | **Round A** — group explore (one live demo) | 20 | 🟢 activity |
| — | *break* | (pause the clock) | 10 | — |
| 6 | [`B0-how-it-works.html`](B0-how-it-works.html) | **Round B** — jigsaw controller | 40 | 🟢 activity |
| 7 | [`B1-agui.html`](B1-agui.html) | Round B card · AG-UI | 0\* | 🟢 activity |
| 8 | [`B2-a2ui.html`](B2-a2ui.html) | Round B card · A2UI | 0\* | 🟢 activity |
| 9 | [`B3-mcp.html`](B3-mcp.html) | Round B card · MCP Apps | 0\* | 🟢 activity |
| 10 | [`C1-plan-your-app.html`](C1-plan-your-app.html) | **Round C** — plan + reflect | 25 | 🟢 activity |
| 11 | [`show-and-tell.html`](show-and-tell.html) | Share | 20 | 🟡 draft |
| 12 | [`07-wrap-up.html`](07-wrap-up.html) | Wrap-up + Q&A | 10 | 🟡 draft |
| 13 | [`08-built-on-this.html`](08-built-on-this.html) | Showcase (AIPLA + GDE) | 4 | 🟢 drafted |
| 14 | [`contact-mark/contact-mark.html`](contact-mark/contact-mark.html) | Close — standard | 1 | ✅ reused |

**Budget:** 160 min on the clock + 10-min break (paused) ≈ **170 min**, leaving
buffer inside a 3-hour half-day.

\* Round B is a **jigsaw**: groups each do *one* protocol in parallel, so the
40-min round budget sits on `B0`; `B1`–`B3` are reference cards (0 min) the
facilitator can project per track.

## Timekeeping (the organising tool)

The presenter ships a per-deck timer + schedule tracker — use it to run the room:

- **Wall clock, top-right:** click to **start/pause**, double-click to reset.
- **Bottom bar:** `Block M:SS / budget` with a colour bar (green → amber at 80% →
  orange over), `Total M:SS / 170:00`, and a **±delta pill** (green = ahead,
  orange = behind). Budgets are the `minutes` above.
- **Break:** click the clock to **pause** for the 10-min break so the delta stays
  honest, then click again to resume.
- **Round B:** start the timer when groups begin; it banks 40 min against `B0`
  while you flip to `B1`/`B2`/`B3` to show a track (those read 0:00 — expected).

## Demo thread — two real apps, used throughout

Each protocol is shown as the clean demo skill, then the same protocol shipping in
a real fork of the platform; in Round B groups **reconstruct** a piece of one.

- **AIPLA** — *AI in Physics Learning & Assessment* ([cphu-aipla-app](https://github.com/sunholo-data/cphu-aipla-app)) ·
  `https://aipla-v01-frontend-wgwhd7mspa-lz.a.run.app/teacher/classes/0b99f4e04792`
- **GDE AP Agent** — *invoice / accounts-payable* ([gde-ap-agent](https://github.com/sunholo-data/gde-ap-agent)) ·
  `https://gde-ap-agent-blqtqfexwa-ew.a.run.app/`

| Protocol | Clean demo skill | Real app | Round B reconstruct |
|---|---|---|---|
| AG-UI | `demo-researcher` | GDE pipeline visualizer · AIPLA tutor | restore `onTextMessageContent` (`B1`) |
| A2UI | `demo-form-builder` → `demo-workspace` | GDE `InvoiceHeroCard` · AIPLA workspace | restore `surface_id="workspace"` (`B2`) |
| MCP Apps | `demo-map-explorer` | GDE dashboards (bidirectional) · AIPLA `led-planck` | restore `onUpdateModelContext` (`B3`) |

> **Live-demo caveat:** demo skills run locally (`LOCAL_MODE=1`); the real apps
> are on Cloud Run and the AIPLA teacher link may need sign-in. Decide per moment
> whether to click through live or show a capture.

## Status legend

- ✅ **reused** — standard book-end deck from `sunholo-data/presentations`.
- 🟢 **activity / drafted** — real content; refine copy.
- 🟡 **draft** — framing decks; expand as needed.

The **faded-code reconstruct exercises** (Round B) still need the actual
"solution removed" branches prepared in the platform/app repos — the cards link
to the *completed* files; someone has to author the blanked-out starting point.
