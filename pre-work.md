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

## Troubleshooting

| Symptom | Fix |
|---|---|
| Chat sends but **nothing streams** / a "model" or auth error | Almost always a missing key — check `GEMINI_API_KEY` is set, then restart `make dev-local`. |
| (Local) Wrong Node/Python version | Node 20+, Python 3.11–3.13; make sure your shell points at them. |
| (Local) Windows: `make` not found / scripts fail | Use **Option A (Codespaces)**, or run inside **WSL2 / Ubuntu** — not PowerShell/CMD. |
| (Local) Port in use (3456 / 1956 / 3457) | Quit the other process (`lsof -i :3456`) and re-run. |
| (Codespaces) Setup seemed to fail | Open the **Terminal → "Codespaces: View Creation Log"**; re-run `bash .devcontainer/post-create.sh`. |

Still stuck? **Arrive a few minutes early** with the error on screen — instructors
can triage before activities start.

---

See also: [agenda2.md](agenda2.md) (running order) and
[facilitator-guide.md](facilitator-guide.md) (running a table). Code is public at
[build-ai-uis-workshop-app](https://github.com/sunholo-data/build-ai-uis-workshop-app)
(a copy of the template
[ai-protocol-platform](https://github.com/sunholo-data/ai-protocol-platform)).
