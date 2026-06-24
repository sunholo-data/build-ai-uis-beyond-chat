# Slides — Build AI UIs Beyond Chat

Self-contained HTML slide decks for the workshop talk. No build step: each deck
is one HTML file with inlined CSS/JS and CDN fonts/icons. Open any file in a
browser, or play the whole talk through the presenter.

## Run it

```bash
# From the repo root — any static server works; this avoids file:// CORS quirks
python3 -m http.server 8000
# then open:
#   http://localhost:8000/slides/presenter.html   ← the full talk (recommended)
#   http://localhost:8000/slides/03-adk-agui.html ← a single deck, standalone
```

- **← / →** — navigate slides within the current deck
- **↑ / ↓** — jump between decks (presenter only)
- **Space** — next slide · **theme toggle** top-right (dark/light, synced across decks)
- **Presenter timer:** click the wall-clock to start/pause, double-click to reset.
  Per-deck budgets come from [`outline.md`](outline.md).

## Layout

```
slides/
  presenter.html          # playlist navigator + per-deck timer — open this for the talk
  outline.md              # block → deck → minutes (source of truth for the playlist)
  00-welcome.html         # cold-open title deck
  01-chat-wall.html …     # one skeleton deck per agenda block (see outline.md)
  07-a2a-close.html
  about-mark/             # standard intro deck (reused from sunholo-data/presentations)
  contact-mark/           # standard close deck (reused)
  images/
    logos/                # sunholo + ailang logos (referenced by every deck)
    case-studies/         # imagery used by the reused book-end decks
```

Talk decks live at `slides/` root and reference logos as `images/logos/…`.
The reused book-end decks sit in their own subfolders and reference
`../images/logos/…` — both resolve to `slides/images/logos/`, so the reused
decks work unchanged.

## Authoring

These decks follow the **`presentation-slides`** skill — see
[`../.claude/skills/presentation-slides/SKILL.md`](../.claude/skills/presentation-slides/SKILL.md)
for the full design system, slide-type patterns, and the copy-paste boilerplate.
Key rules:

- **16:9 stage, 1920×1080.** Size everything inside a slide with `cqi`/`cqb`
  (container query units) or `%`/flex — **never** `vh`/`vw`. The only allowed
  viewport unit is the one shell line `#app{…;height:100vh}`.
- **Both themes** must look polished (dark default + `[data-theme="light"]`).
- When you add/remove a slide: update `const TOTAL`, `#slideTotal`, and add any
  new animated element classes to the reset selector in `goTo()`.

Lint a deck for stage-safe sizing:

```bash
node .claude/skills/presentation-slides/scripts/lint-slides.mjs slides/*.html
```

The skeleton decks were generated to share identical CSS; expand the content
slide-by-slide. The `.note` line on each block's last slide points back to the
matching section of [`../agenda.md`](../agenda.md).

## Publishing (not enabled yet)

The decks are plain static files, so they can ship to GitHub Pages. The source
`presentations` repo deploys via a `.github/workflows/deploy.yml` that uploads
the repo root. This repo has **no Pages workflow** — adding one is a deliberate,
outward-facing step (it enables Pages on push). Wire it only when the decks are
ready to be public.
