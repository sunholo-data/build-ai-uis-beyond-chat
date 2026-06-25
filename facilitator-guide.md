# Facilitator guide — how to actually run the room

> Field notes, not theory. This is the **instructor playbook** for the
> half-day (~3h) activity-led workshop. [agenda2.md](agenda2.md) is the
> running order (the *what*); [slides/outline.md](slides/outline.md) is the
> deck order + per-deck minute budgets that drive the presenter's timer.
> This page is the *how* — what to say, where groups get stuck, what to cut
> when you're behind.

The shape of the day: **instructor intro → 3 student activity rounds → share
→ wrap**, in groups of 3–5. AI coding is encouraged. A **helper agent**
fields FAQs and rates submissions so you can circulate instead of grading.

---

## 1. Before the room (pre-flight)

Run this checklist the morning of. None of it is optional — the failure modes
are all silent and all cost you the first 20 minutes.

| Check | Done when | If it's not |
|---|---|---|
| **Pre-work landed** | Attendees arrived with `make dev-local` green on the fork's `workshop-start` branch (see [pre-work.md](pre-work.md)) | Triage on arrival — see §6. Don't let a broken laptop stall a table. |
| **Fork pinned** | Cohort cloned the pinned tag, not the moving template ([workshop-fork.md](workshop-fork.md)) | A drifting template means your reveal diffs won't match their files. |
| **Helper agent up** | It answers a test FAQ and rates a throwaway submission to the workspace pane ([helper-agent-design.md](helper-agent-design.md)) | The whole show-and-tell (§8) depends on it. Have a fallback: you read out 2–3 scores manually. |
| **Join code printed** | The anonymous-group join code is on a slide *and* on paper/whiteboard | No accounts = no friction. If the code's wrong, the room can't connect. |
| **Demo backups ready** | Screen-capture of each demo on disk | The **AIPLA teacher link needs sign-in**; the **MCP map is cloud-only**. Decide *before* the moment whether you click through live or play the capture. |
| **Room layout** | Tables of 3–5, each with power + a screen they can share | Solo rows kill the jigsaw. |
| **AV** | Presenter on the big screen, your laptop free to circulate | You'll be walking the room in the rounds — don't tether yourself to the podium. |

**The two demos that will bite you:** the AIPLA teacher view ($link$ in
[slides/outline.md](slides/outline.md)) gates behind sign-in, and the MCP
Cesium map runs cloud-only (`LOCAL_MODE` won't save you). Both have captures.
Use them without apology if the live click is risky — the point is the
*protocol behaviour*, not your network.

---

## 2. Running the timer

The presenter ([slides/outline.md](slides/outline.md) describes it; it's
`slides/presenter.html`) is your stopwatch and your conscience.

- **Click the wall clock (top-right) to start/pause.** Start it at "Welcome."
- **Bottom bar** shows `Block M:SS / budget` (green → amber at 80% → orange
  over), `Total / 170:00`, and a **±delta pill**: green = ahead, orange =
  behind. Glance at the pill, not the wall.
- **PAUSE during the 10-min break.** Click the clock when groups stand up;
  click again when they sit. If you forget, the delta lies for the rest of
  the day and your cut decisions (§9) go wrong.
- **Round B is a jigsaw:** the 40-min budget banks against `B0`. When you flip
  to `B1`/`B2`/`B3` to show a track they read `0:00` — that's expected, not a
  bug.
- Budgets are owned by [slides/outline.md](slides/outline.md). If you retime
  the day, change them there and keep the presenter's `PLAYLIST` in sync.

---

## 3. Per-block run notes

Block by block. Times are the [agenda2.md](agenda2.md) running order.

### Welcome / how-today-works / real-apps teaser — 5 min · instructor
Set the deal in one breath: **"⅔ of today is you building, ⅓ is me framing.
You'll be in groups, AI coding is fine, and that agent over there answers your
questions and grades your work."** Flash the two real apps (AIPLA, GDE) for
five seconds each — *"this ships"* — then move. Don't demo yet.

### About Mark — 2 min · instructor
Standard book-end deck. Keep it to two minutes; the room came to build.

### Chat wall + Brain break #1 — 8 min · instructor + pair
Show the chat wall (the everything-is-a-text-box problem). Then the first
brain break: **"turn to your neighbour — which non-chat AI tools would help in
*your* work?"** Give it the full 2 minutes; harvest 2–3 answers out loud.
This is the hook for the whole day, not filler.

### Guided tour: AG-UI / A2UI / MCP — 25 min · instructor + brain breaks
**You** show each protocol's with/without contrast — groups do **not** run two
versions (too heavy to set up per table). ~6 min per protocol, each capped by
a 2-min pair talk so no passive stretch runs long. End on **Brain break #2:
"explain the difference between AG-UI, A2UI & MCP Apps to your neighbour."**
That teach-back is the pre-load for the Round B jigsaw — if they can't do it
here, slow down before you turn them loose.

> This block is your **biggest compressible chunk** (§9). If the morning ran
> late, trim a contrast, not a round.

### Round A — Explore a live skill — 20 min · groups
Each table picks **ONE** running demo, pokes it, and lists **3 things the
protocol enables that plain chat can't**. You circulate. Last ~5 min: a fast
share across tables — one line each, no laptops. Goal is articulation, not
code.

### Break — 10 min · PAUSE THE CLOCK
Say "ten minutes" and mean it. Pause the presenter clock (§2).

### Round B — Reconstruct (jigsaw) — 40 min · groups
The centrepiece. Full mechanics in §4. Two phases: 20-min expert groups (each
table restores ONE protocol), then regroup for 15-min teach-backs (~3 min
each). Protect this block.

### Round C — Plan/prototype your own — 25 min · groups
Groups sketch a protocol app for *their own* context (lesson tool, intake
form, dashboard) and reflect. Hand them the skeleton skill
([workshop-fork.md](workshop-fork.md) → Round C) — fork, change the
instruction + one tool, run. **This is the round you protect hardest** (§9):
it's where the day pays off.

### Show & tell — 20 min · groups present
Groups upload to the helper agent; it rates live (§8). ~2–3 min per group.
You MC, you don't grade.

### Wrap-up + Q&A — 10 min · instructor
Synthesis + the meta-reveal: **the helper agent they just used is built on all
three protocols** ([helper-agent-design.md](helper-agent-design.md)). Then the
4 advanced patterns and Q&A.

### Showcase + contact — ~5 min
AIPLA + GDE as "where this goes," then contact. First thing to cut if behind.

---

## 4. The jigsaw in detail (Round B)

The jigsaw only works if you set it up *before* groups start. Decide the
assignment during the break.

**Phase 1 — expert groups (20 min).** Each table does exactly **one**
protocol. Split the tables into **roughly equal thirds**: ~⅓ on AG-UI (B1),
~⅓ on A2UI (B2), ~⅓ on MCP (B3). Write the assignment on the board so nobody
drifts. Project that track's card (`B1`/`B2`/`B3`) for the assigned tables.

**Phase 2 — regroup + teach-back (15 min).** Re-mix so **each new group has
one expert per protocol.** Easiest way to do it live:

1. Within each Phase-1 table, **number off** 1, 2, 3, …
2. Call: *"all the 1s to that corner, 2s here, 3s there."*
3. Each new cluster should now hold an AG-UI, an A2UI, and an MCP expert.
4. Round-robin **~3-min teach-backs**: each expert shows the others what they
   restored and the success check that proved it.

**Keep teach-backs to 3 min.** Call time out loud ("switch!") every 3
minutes — three experts × 3 min ≈ 9–10 min, leaving slack to regroup. If a
group is short an expert (uneven thirds), pair two of the missing protocol's
experts into one cluster; coverage matters more than symmetry.

---

## 5. Per-exercise stuck-points + hints (Round B)

Each is one observable edit. Success checks below are from
[workshop-fork.md](workshop-fork.md). First line of support is always **the
helper agent** and **[protocol-gotchas.md](protocol-gotchas.md)** — point,
don't solve. Reveal of last resort: `git diff workshop-start main -- <file>`.

### B1 · AG-UI — restore the event stream
- **File:** `frontend/src/hooks/useSkillAgent.ts` (the `agent.subscribe({…})`
  block). **Restore** the `onTextMessageContent` handler that appends each
  delta to the in-progress assistant message.
- **✅ Done when:** messages stream **token-by-token**; events visible in
  **DevTools → Network → `stream`**.
- **Stuck on:** "nothing streams" usually = the handler is restored but they
  didn't reload, or they're watching the wrong message. Point them at the
  `stream` entry in Network — if events flow but text doesn't, the append is
  wrong. Gotcha to mention: tool-only turns can render blank
  ([protocol-gotchas.md](protocol-gotchas.md) #4).
- **Reveal:** `git diff workshop-start main -- frontend/src/hooks/useSkillAgent.ts`

### B2 · A2UI — send UI to the workspace surface
- **File:** the demo skill config (`tool_configs.a2ui`). **Restore**
  `surface_id = "workspace"` so the UI declares a surface instead of falling
  back to the chat bubble.
- **✅ Done when:** *"make me a contact form"* renders in the **workspace
  pane**, not inline in the chat bubble.
- **Stuck on:** form renders in the chat bubble = `surface_id` still missing
  or misspelled. If it renders **nowhere**, that's the v0.9-vs-v0.8 import
  trap ([protocol-gotchas.md](protocol-gotchas.md) #5) — a good stretch for a
  fast group, but flag it so they don't chase a phantom.
- **Reveal:** `git diff workshop-start main -- <skill-config file>` (confirm
  the path against the fork; it's the demo workspace skill config).

### B3 · MCP Apps — wire the second RPC channel
- **File:** the frontend MCP-app renderer (the `onUpdateModelContext` /
  `onFallbackRequest` dispatch), paired with the backend iframe-context
  endpoint. **Restore** the `onUpdateModelContext` handler so the widget's
  state merges into the agent's next-turn context.
- **✅ Done when:** show a map, ask *"what city is currently centred?"* — the
  agent answers **from context, with NO re-render** (it doesn't call
  `show-map` again).
- **Stuck on:** the agent re-renders the map instead of answering = the
  handler isn't wired, so the iframe state never reaches the next turn. This
  is the **hardest of the three** — there are two RPC channels, not one
  ([protocol-gotchas.md](protocol-gotchas.md) #9). Remind them the map will
  render fine even when the handler is missing; the tell is the agent staying
  *blind to its own UI*. Note this exercise needs the cloud map (no
  `LOCAL_MODE`), so a table on B3 must be online.
- **Reveal:** `git diff workshop-start main -- <renderer file>`

> Difficulty is roughly matched so groups finish together, but **B3 runs
> longest**. If you assigned uneven thirds, give B3 the more confident tables.

---

## 6. Setup triage (broken `make dev-local`)

A red `make dev-local` on arrival is the single most common time-sink. The
rule: **never debug one laptop while a group waits.**

1. **Pair them onto a working table.** Two people, one green instance is fine
   — better than one person stuck on a red one. The activities are group work
   anyway.
2. **Keep a spare working instance** running on your own machine (or a backup
   laptop) that you can hand to a stranded solo attendee.
3. **Batch the fixes.** If 3+ people hit the *same* error, fix it once for the
   room (likely a missed pre-work step — [pre-work.md](pre-work.md)). If it's
   one weird laptop, don't chase it during the rounds; pair and move on.
4. **The helper agent doesn't need their local stack.** Even a fully broken
   laptop can join the anonymous group and submit via paste-in-chat. Nobody is
   locked out of show-and-tell.

---

## 7. Large-crowd / group formation

Everything already runs in groups, so scale is mostly a formation problem.

- **Tables of 3–5.** Below 3 the jigsaw teach-back is thin; above 5 someone
  spectates.
- **Form groups at "Welcome,"** not at Round A — you don't want shuffling
  eating activity time.
- **Mix skill levels** per table so AI-coding-confident and not-so-confident
  sit together. A table that's all beginners stalls in Round B; all experts
  finish Round C in five minutes and get bored.
- **For the jigsaw, plan the thirds during the break** (§4) — count tables,
  divide by 3, write it on the board before Round B starts.
- **8 groups is the design point** for the helper-agent rating (§8). More than
  that, lengthen show-and-tell or batch the readouts.

---

## 8. Using the helper agent to feed back 8 groups fast

You can't grade 8 groups in 20 minutes. You don't have to —
[helper-agent-design.md](helper-agent-design.md) does it.

- Groups **submit** (paste a diff/snippet — default — or a branch link). The
  agent calls `rate_submission`, grades **server-side** against a per-track
  rubric, and renders an **A2UI scorecard** to the workspace pane while
  **streaming** the critique over AG-UI.
- **Rubric, 0–3 each:** Works · Correct · Idiomatic · (Spark, Round C only).
- **Your job is MC, not judge.** Let the scorecard land, then add *one* human
  sentence per group — colour the agent can't give ("loved that you picked an
  intake form for your actual admissions flow").
- **Keep it encouraging, not gatekeeping.** The score exists to give 8 groups
  fast, specific feedback at once — not to rank people. If the agent's tone
  drifts harsh, say so out loud and reframe.
- **Submissions roll into a live leaderboard/gallery** in the workspace — that
  *is* your show-and-tell surface; no slide-flipping.
- **Fallback:** if the rater is down, have groups paste into chat and you read
  out 2–3 scores by eye against the same rubric. Don't cancel the share.

> The meta-payoff for the wrap-up: the thing that just graded them is built on
> AG-UI + A2UI + MCP. Say it out loud.

---

## 9. Timing reality + cut order

You will run behind. Decide the cuts *before* you need them, and protect the
parts that make the day worth attending.

**Protect, in order:**
1. **Round C** (prototype your own) — the payoff. Cut this and people leave
   with nothing of their own.
2. **Round B** (reconstruct + jigsaw) — the core learning.
3. The break — short it to 7 min if you must, but don't skip it (and re-pause
   the clock correctly, §2).

**Compress / cut, in order (do these first):**
1. **Showcase** (AIPLA + GDE at the end) — drop to a single slide or skip.
2. **The guided tour** — trim a with/without contrast; 25 min → 18 is fine if
   Brain break #2 still works.
3. **Round A share-out** — keep the poking, cut the cross-table readout.
4. **Show & tell** — fewer groups present live; the rest still get a scorecard
   from the helper agent, just not stage time.

**Reading the day off the delta pill:** orange and climbing through the tour =
start trimming contrasts *now*, not at the break. If you hit the break already
behind, the break is where you decide whether the Showcase survives. Round B
and Round C are not on the table.

---

## Companion docs

- [agenda2.md](agenda2.md) — running order + pedagogy (the *what*).
- [slides/outline.md](slides/outline.md) — deck order + minute budgets (the
  presenter's timer source of truth).
- [workshop-fork.md](workshop-fork.md) — the pinned fork, the three blanked
  exercises, the rating flow.
- [pre-work.md](pre-work.md) — what attendees must do before arriving (`make
  dev-local` green).
- [helper-agent-design.md](helper-agent-design.md) — the FAQ + rating agent.
- [protocol-gotchas.md](protocol-gotchas.md) — the bear traps, mapped to
  blocks — your first answer to most "why isn't this working."
