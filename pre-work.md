# Pre-work — get the app running BEFORE you arrive

> Do this 24–48h before the workshop. It takes about 15 minutes. The one
> thing that matters: **arrive with the app running locally.** Everything
> else is a nicety.

## Why this matters

This is an activity-led, half-day workshop, and you'll work in groups at a
table. The build rounds assume everyone already has the app up — there's no
30-minute install break to hide a broken setup in. If you turn up with
nothing running, you stall your whole table while an instructor triages your
laptop instead of teaching. Get past `make dev-local` at home, on your own
network, with time to spare.

## 1. Install

You need these on your laptop:

- **Node 20+** — check with `node --version`
- **Python 3.11+** — check with `python3 --version`
- **git** — check with `git --version`
- **A GitHub account** — you'll clone (and later own a fork of) the code

Optional but encouraged:

- **An AI coding tool** — Claude Code, Cursor, or Copilot. You'll use AI
  coding during the build rounds, and it's a lot more fun if your tool of
  choice is already set up and signed in.

## 2. Get the code

Clone the pinned workshop fork and check out the `workshop-start` branch —
that branch is where the exercises live.

```bash
git clone https://github.com/sunholo-data/build-ai-uis-workshop-app.git
cd build-ai-uis-workshop-app
git checkout workshop-start
```

> ⚠️ The fork name is still being finalised. If that clone **404s**, fall
> back to the public template and use its default branch:
>
> ```bash
> git clone https://github.com/sunholo-data/ai-protocol-platform.git
> cd ai-protocol-platform
> ```

## 3. Run it

One command, in LOCAL_MODE — pure Node + Python, no containers. `LOCAL_MODE=1`
stubs out Firestore and auth so nothing external is required.

```bash
make dev-local
```

It starts three things:

| What | URL / port |
|---|---|
| Frontend | http://localhost:3456 |
| Backend API | http://localhost:1956 |
| MCP sandbox (optional) | http://localhost:3457 |

## 4. Verify you're ready

Open **http://localhost:3456**, then run the streaming check:

1. You see a **yellow LOCAL_MODE banner** at the top of the page.
2. Type a message into the chat and send it.
3. The reply **streams in token-by-token** (not all at once at the end).

If it streams, you're ready. That's the whole test.

**You're ready when…**

- [ ] `node --version` is 20 or higher
- [ ] `python3 --version` is 3.11 or higher
- [ ] You cloned the repo and are on the `workshop-start` branch
- [ ] `make dev-local` runs without errors
- [ ] http://localhost:3456 shows the yellow LOCAL_MODE banner
- [ ] A chat message streams back token-by-token

## 5. You do NOT need

- ❌ **Docker** — LOCAL_MODE runs straight on Node + Python
- ❌ **GCP credentials** — nothing talks to Google Cloud locally
- ❌ **A Firebase account** — auth and Firestore are stubbed

If a setup step asks you for any of these, you're off the LOCAL_MODE path —
stop and re-check section 3.

## 6. Troubleshooting

| Symptom | Fix |
|---|---|
| Wrong Node version | Install Node 20+ (use `nvm install 20` if you have nvm). Re-check with `node --version`. |
| Wrong Python version | Install Python 3.11+. Re-check with `python3 --version`. Make sure your shell points at the new one. |
| Port already in use (3456 / 1956 / 3457) | Something else is on that port. Quit the other process (or find it: `lsof -i :3456`), then re-run `make dev-local`. |
| `make dev-local` errors | Read the first error, not the last — it's usually a missing Node or Python dep. Re-run after installing it. |
| Chat sends but nothing streams | Both halves must be up. Confirm the **frontend** at http://localhost:3456 **and** the **backend** at http://localhost:1956 are both running — a blank stream usually means the backend died or never started. |

Still stuck after all that? **Arrive a few minutes early.** Instructors can
triage a broken laptop before we start — but not once the activities are
running. Come with the error message on screen.

---

See also: [agenda2.md](agenda2.md) for the running order, and
[facilitator-guide.md](facilitator-guide.md) if you're helping run a table.
The platform code is public at
[sunholo-data/ai-protocol-platform](https://github.com/sunholo-data/ai-protocol-platform).
