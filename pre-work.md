# Pre-work — get the app running BEFORE you arrive

> Do this 24–48h before the workshop. ~15 minutes. The one thing that matters:
> **arrive with the app running and a chat reply streaming back.**

## TL;DR

- **Easiest, any OS (Windows/Mac/Linux/Chromebook), nothing to install →** open the
  repo in **GitHub Codespaces**.
- **Prefer it on your own machine →** local setup (macOS/Linux, or Windows via WSL2).
- **Either way, get a free Gemini key** (2 min): <https://aistudio.google.com/apikey>
  — the LLM isn't stubbed, so without it the chat won't reply. (Free tier; *not* a
  GCP project.)
- **Bringing a coding agent for Round C?** Log it in *before* you arrive — a
  subscription/OAuth login beats a metered API key for a long session. See
  **Set up your AI coding assistant** below.

You do **not** need Docker, a GCP account, or Firebase.

## Why this matters

This is an activity-led, half-day workshop and you'll work in groups at a table.
The build rounds assume everyone already has the app up — there's no 30-minute
install break. Turn up with nothing running and you stall your whole table.

---

## Option A — Codespaces (recommended, any OS)

Runs the whole thing in the cloud from your browser or VS Code. Nothing to
install locally; everyone gets an identical environment. Needs only a (free)
GitHub account.

1. Go to **<https://github.com/sunholo-data/build-ai-uis-workshop-app>**
2. Green **Code** button → **Codespaces** tab → **Create codespace on main**.
3. Wait ~3–5 min the first time — it auto-installs make, uv, and all deps.
4. In the Codespace terminal, add your key (from the link above):
   ```bash
   echo "GEMINI_API_KEY=your-key-here" > backend/.env
   ```
   *(Or, to avoid pasting it each time: GitHub → Settings → Codespaces → Secrets,
   add `GEMINI_API_KEY` scoped to this repo.)*
5. Start it:
   ```bash
   make dev-local
   ```
6. Port **3456** pops open automatically → see the **yellow LOCAL_MODE banner** →
   send a message → it **streams**. Done.

> A 3-hour session is well within the free Codespaces tier. Create the Codespace a
> day early so the first-time setup isn't a surprise on the morning.

---

## Option B — Local setup

### Install

- **Node 20+** — `node --version` (this also gives you `npm`)
- **Python 3.11–3.13** — `python3 --version` (3.14 isn't supported yet)
- **git** — `git --version`
- **make** — `make --version`
- **A GitHub account**

You do **not** need to install `uv` separately — `make install` fetches it.

**Per-OS:**
- **macOS** — `xcode-select --install` for `make` + `git`; Node/Python via
  [Homebrew](https://brew.sh) (`brew install node python git`).
- **Linux** — `sudo apt install build-essential git`, plus Node 20+ / Python 3.11+.
- **Windows** — easiest is **Option A (Codespaces)**. If you want it local, use
  **WSL2** (`wsl --install` in admin PowerShell → reboot → Ubuntu) and follow the
  Linux steps inside it; native PowerShell/CMD won't run the `make`/bash tooling.

### Run

```bash
git clone https://github.com/sunholo-data/build-ai-uis-workshop-app.git
cd build-ai-uis-workshop-app
cd backend && make install && cd ..       # installs uv + Python deps (~1–2 min)
cd frontend && npm install && cd ..        # installs Node deps
echo "GEMINI_API_KEY=your-key-here" > backend/.env
make dev-local                             # backend :1956 + frontend :3456
```

---

## Verify you're ready (either option)

Open **http://localhost:3456** (Codespaces opens the forwarded URL for you):

1. You see a **yellow LOCAL_MODE banner** at the top.
2. Send a message in the chat.
3. The reply **streams in token-by-token**.

**You're ready when…**

- [ ] The app loads with the yellow **LOCAL_MODE** banner
- [ ] `backend/.env` has your `GEMINI_API_KEY` (or the Codespaces secret is set)
- [ ] A chat message **streams** back

---

## Set up your AI coding assistant (recommended)

> Everything above gets the **app** running — that's the required bit. This is about
> the **coding agent you'll build with** in Round C, which is a *separate* login. The
> exercises are all completable by hand (every blank has a `git diff` reveal), so the
> agent is an **accelerant, not a hard requirement** — but it's where the fun is.

**Two different keys — don't confuse them.** The `GEMINI_API_KEY` from above powers the
**app's** chat. Your coding assistant (below) is the thing that writes code *for* you —
a different account and a different login.

### Pick one and log in

| Tool | Sign-in | Cost model | Free path? |
|---|---|---|---|
| **Claude Code** | OAuth (Claude Pro/Max) **or** API key | subscription = flat; API key = metered | — (paid) |
| **OpenAI Codex** | OAuth (ChatGPT Plus/Pro) **or** API key | subscription = flat; API key = metered | — (paid) |
| **Gemini CLI** | Google account (OAuth) **or** API key | generous free tier on a personal login | ✅ free tier |
| **Antigravity** | Google account | free during preview | ✅ free |

**Strongly prefer a subscription / OAuth login over a metered API key.** A 3-hour
agentic session on per-token billing (especially on the biggest models) can quietly run
into real money; a flat subscription removes the bill-shock. On an API key only? Set a
spend cap and default to a cheaper model.

**No subscription? You're not locked out.** Gemini CLI's free OAuth tier and Antigravity's
free preview are genuinely zero-cost and plenty for the build rounds.

### The skills work across all of them

The fork ships pre-built **skills** (`SKILL.md`) your agent loads on demand. They follow
the open Agent Skills standard, so the *same* skills fire in Claude Code, Codex, **and**
Gemini CLI — no Claude-only setup needed.

### Smoke-test: does your agent see the skills?

The thing that silently fails is **discovery** — the skill sits in the repo but the agent
never loads it. Confirm before you arrive:

1. Open the cloned fork in your coding assistant.
2. Run the smoke-test prompt from the repo's `README` (or just ask *"which workshop
   skills are available?"*).
3. The agent should pick up a skill's instructions by name — not answer cold.

**You're ready (coding agent) when…**

- [ ] Your assistant is **logged in** (subscription/OAuth preferred)
- [ ] You've opened the **fork** in it
- [ ] A **skill fires** on the smoke-test prompt

## Troubleshooting

| Symptom | Fix |
|---|---|
| Chat sends but **nothing streams** / a "model" or auth error | Almost always a missing key — check `GEMINI_API_KEY` is set, then restart `make dev-local`. |
| (Local) Wrong Node/Python version | Node 20+, Python 3.11–3.13; make sure your shell points at them. |
| (Local) Windows: `make` not found / scripts fail | Use **Option A (Codespaces)**, or run inside **WSL2 / Ubuntu** — not PowerShell/CMD. |
| (Local) Port in use (3456 / 1956 / 3457) | Quit the other process (`lsof -i :3456`) and re-run. |
| (Codespaces) Setup seemed to fail | Open the **Terminal → "Codespaces: View Creation Log"**; re-run `bash .devcontainer/post-create.sh`. |
| (Coding agent) A skill isn't picked up | Confirm it's in the dir your tool scans — `.claude/skills` (Claude Code) vs `.agents/skills` (Codex / Gemini CLI) — then restart the session. |

Still stuck? **Arrive a few minutes early** with the error on screen — instructors
can triage before activities start.

---

See also: [agenda.md](agenda.md) (running order) and
[facilitator-guide.md](facilitator-guide.md) (running a table). Code is public at
[build-ai-uis-workshop-app](https://github.com/sunholo-data/build-ai-uis-workshop-app)
(a copy of the template
[ai-protocol-platform](https://github.com/sunholo-data/ai-protocol-platform)).
