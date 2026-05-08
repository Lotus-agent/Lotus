<div align="center">

# 🌸 Lotus

### **Autonomous AI Control Agent for Windows & macOS — driven from Telegram, powered by local AI.**

[![Windows](https://img.shields.io/badge/Windows-v2.2.0--STABLE-0078D4?style=for-the-badge&logo=windows&logoColor=white)](https://github.com/SatyamPote/Lotus/releases)
[![macOS](https://img.shields.io/badge/macOS-v2.0.1-000000?style=for-the-badge&logo=apple&logoColor=white)](https://github.com/SatyamPote/Lotus/releases)
[![Python](https://img.shields.io/badge/Python-3.13-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org)
[![Swift](https://img.shields.io/badge/Swift-6.0-F05138?style=for-the-badge&logo=swift&logoColor=white)](https://swift.org)
[![Telegram](https://img.shields.io/badge/Telegram-Bot-26A5E4?style=for-the-badge&logo=telegram&logoColor=white)](https://telegram.org)
[![Ollama](https://img.shields.io/badge/Ollama-Local%20AI-000000?style=for-the-badge)](https://ollama.com)

**Lotus turns your computer into an AI-controlled remote workstation.**
Send natural-language commands from Telegram, and Lotus will operate your
desktop — opening files, playing music, taking screenshots, running research,
and chatting with you using a private, local LLM. No cloud, no data leakage,
no surprises.

</div>

---

## Table of Contents

- [What is Lotus?](#what-is-lotus)
- [Quick Start](#-quick-start)
- [Feature Tour](#-feature-tour)
- [Command Reference](#%EF%B8%8F-command-reference)
- [Installation](#-installation)
  - [Windows (v2.2.0-STABLE)](#-windows-v220-stable)
  - [macOS (v2.0.1)](#-macos-v201)
- [Architecture](#-architecture)
- [Configuration](#%EF%B8%8F-configuration)
- [Privacy & Security](#-privacy--security)
- [Troubleshooting](#-troubleshooting)
- [Development](#%EF%B8%8F-development)
- [Releases & CI/CD](#%EF%B8%8F-releases--cicd)
- [Repository Layout](#-repository-layout)
- [Roadmap](#%EF%B8%8F-roadmap)
- [Contributors](#-contributors)
- [License](#-license)

---

## What is Lotus?

**Lotus** is a cross-platform AI agent that bridges your computer and Telegram.
It runs as a background service, listens to messages from a small set of
allowed Telegram users, interprets them, and translates them into real
actions on your machine — file lookups, app automation, music playback,
screen capture, deep research, and conversational AI.

The intelligence layer is fully local: Lotus integrates with
[**Ollama**](https://ollama.com) to run open-weight LLMs (Llama, Qwen,
Phi, etc.) on your own hardware. **Nothing about your files, your queries,
or your conversations leaves the machine** — except, of course, the
Telegram messages you choose to send.

Two flavors share the same philosophy and the same MCP-style tool surface,
but each is implemented natively for its platform:

| | Windows | macOS |
|---|---|---|
| **Latest version** | v2.2.0-STABLE | v2.0.1 |
| **Installer** | `LotusSetup.exe` (Inno Setup, autonomous) | `Lotus-2.0.1.dmg` (drag-to-install) |
| **GUI** | Tkinter / customtkinter control panel | Native Swift menu-bar app (`Lotus.app`) |
| **Service** | Background `pythonw.exe` process | `launchd` user agent (`com.lotus.botservice`) |
| **Source** | [`Windows-MCP/`](Windows-MCP) | [`Mac-MCP/`](Mac-MCP) |
| **Lead** | [@SatyamPote](https://github.com/SatyamPote) | [@JayashBhandary](https://github.com/JayashBhandary) |

---

## ⚡ Quick Start

> *Already comfortable with Telegram bots and Ollama? Here's the 60-second version.*

1. **Get a Telegram bot token** from [@BotFather](https://t.me/BotFather) and your Telegram user ID from [@userinfobot](https://t.me/userinfobot).
2. **Install [Ollama](https://ollama.com)** and pull a small model: `ollama pull qwen2.5:3b`.
3. **Install Lotus** for your platform:
   - **Windows** — download `LotusSetup.exe` from [Releases](https://github.com/SatyamPote/Lotus/releases) and run it.
   - **macOS** — download `Lotus-2.0.1.dmg`, drag `Lotus.app` to `/Applications`, launch.
4. **Configure** — paste your bot token, your allowed Telegram user ID, and pick the Ollama model.
5. **Start the bot** — DM your bot. Try `dashboard`, `take screenshot`, or `play lo-fi`.

That's it. The setup wizard handles Python, dependencies, and any platform plumbing automatically.

---

## 🚀 Feature Tour

### ⚖️ Strict Priority Routing
Commands are matched against a hardened priority chain — **System > Files >
Music > Research** — *before* the LLM ever sees them. This guarantees that
literal commands like `open report.pdf` or `volume up` execute deterministically
and don't get hallucinated into something else. The AI is invoked only for
genuinely ambiguous or open-ended queries.

### 🔍 Multi-Source Research Engine
The `research <topic>` command runs a tiered pipeline:

1. **Wikipedia** — primary structured source, fast and citation-friendly
2. **DuckDuckGo Instant Answer API** — fallback for current events
3. **Web scraping with `markdownify`** — final fallback for arbitrary URLs

Results are aggregated, the LLM produces a structured summary, and you receive
a **professional PDF report** plus inline images via Telegram.

### 🗣️ Voice Feedback
Every action emits a spoken confirmation in a clear, natural female voice.
Local TTS plays through your speakers; the same audio is sent as a
**Telegram Voice Note** for remote acknowledgement when you're away from
the machine.

### 📦 Managed Storage with Auto-Cleaning
Lotus reserves a 2 GB sandbox under your user data dir for downloads,
research artifacts, and screen recordings. An LRU cleanup keeps it under
quota — your disk doesn't fill up if you forget about it.

### 🎵 Stable Music System
Single-instance enforcement: only one player is ever alive at a time, so
queueing a new song cleanly stops the previous one.

- `play <query>` — searches and streams via `yt-dlp`
- `pause` / `resume` / `stop`
- `next` / `prev` — through the session queue
- `volume up` / `volume down` — system mixer hooks

### 🤖 Private Local AI (Ollama)
Default model: `qwen2.5:3b` — runs comfortably on a recent MacBook or any
PC with 8 GB RAM. Want bigger? Swap to `llama3.1:8b`, `phi4`, or any model
in the [Ollama library](https://ollama.com/library) — Lotus picks it up
without code changes.

### 📹 Screen & Media Tools
- `take screenshot` — instant PNG of the current desktop
- `record screen <seconds>` — captures video (ffmpeg under the hood)
- `download <youtube-url>` — pulls audio or video via `yt-dlp`

### 🖼️ Polished Telegram UI
Every reply is wrapped in a clean monospaced frame with a header banner.
File listings, dashboards, and research summaries are visually distinct
and pleasant to read on mobile.

---

## 🛠️ Command Reference

A condensed catalog. Type `help` to your bot for an in-chat version.

### 📂 File Management
| Command | What it does |
|---|---|
| `find <query>` | Fuzzy search across user dirs and the storage sandbox |
| `open <filename>` | Open the file in its default application |
| `send <filename>` | Upload the file to Telegram |
| `ls` | List the current working directory |
| `cd <path>` | Change the bot's working directory |
| `tree` | Print a directory tree (depth-limited) |

### 🎵 Media & Music
| Command | What it does |
|---|---|
| `play <song name>` | Search + stream audio |
| `pause` / `resume` / `stop` | Standard playback control |
| `volume up` / `down` | System volume nudge |
| `next` / `prev` | Skip in the session queue |
| `now playing` | Show current track and elapsed time |

### 🔍 Research & Intelligence
| Command | What it does |
|---|---|
| `research <topic>` | Wikipedia → DDG → scrape → PDF |
| `list research` | Most recent reports with timestamps |
| `say <text>` | Speak text via local TTS + Telegram voice note |
| `chat <prompt>` | One-shot LLM completion (Ollama) |

### 🖥️ System Control
| Command | What it does |
|---|---|
| `dashboard` | Battery, CPU, RAM, disk, uptime, IP |
| `lock` / `sleep` / `shutdown` | Power management |
| `take screenshot` | Capture the desktop as PNG |
| `record screen <seconds>` | Capture a screen video |

---

## 📦 Installation

### 🪟 Windows (v2.2.0-STABLE)

Lotus for Windows ships as a single, autonomous Inno Setup installer
(`LotusSetup.exe`). It handles every prerequisite without manual
intervention:

1. Download **`LotusSetup.exe`** from the [Releases](https://github.com/SatyamPote/Lotus/releases) page.
2. Right-click → **Run as administrator** (only required for the first install — needed to register the auto-start hook).
3. Walk through the wizard:
   - **Telegram Bot Token**
   - **Allowed Telegram User ID(s)** — comma-separated for multi-user
   - **Ollama model name** — e.g. `qwen2.5:3b`, `llama3.1:8b`, `phi4`
   - **Storage location** — defaults to `%LOCALAPPDATA%\Lotus`
4. Click **Finish** — Lotus registers itself as a startup task and launches in the system tray.
5. DM your bot to confirm it's alive.

**Source:** [`Windows-MCP/`](Windows-MCP) · **Lead:** [@SatyamPote](https://github.com/SatyamPote)

#### Uninstalling
- **Settings → Apps → Lotus → Uninstall**, or
- Run `Uninstall Lotus.lnk` from the Start menu

The uninstaller cleans up the auto-start task, the storage dir, and all
config keys.

---

### 🍎 macOS (v2.0.1)

Lotus for macOS is a **truly standalone** native menu-bar app. The DMG
contains a universal (`arm64 + x86_64`) `Lotus.app` with everything it
needs to run on a fresh machine — including a bundled `uv` package
manager and the full Python bot runtime template.

> **No `git clone`. No `uv sync`. No Homebrew. Just drag-to-install.**

#### Install steps

1. Download **`Lotus-2.0.1.dmg`** from the [Releases](https://github.com/SatyamPote/Lotus/releases) page.
2. Open the DMG. The window shows `Lotus.app` next to an `Applications` shortcut over a lotus-pond banner.
3. Drag **`Lotus.app`** onto **`Applications`**.
4. Open **`/Applications`** in Finder, right-click `Lotus.app` → **Open** (this satisfies Gatekeeper for the ad-hoc-signed bundle). Subsequent launches don't need this.
5. The 🌸 icon appears in your menu bar. Click it → **Show Lotus**.
6. The first-run wizard provisions Python 3.13 and the bot dependencies into:
   ```
   ~/Library/Application Support/Lotus/runtime/
   ```
   This takes **~30–60 seconds** the first time and **0 seconds** thereafter.
7. Enter your Telegram token, allowed Telegram user IDs, your name, and an Ollama model. Click **Save & Launch Bot**.

#### Optional: silence Gatekeeper without the right-click dance

```bash
xattr -d com.apple.quarantine /Applications/Lotus.app
```

#### What's bundled inside `Lotus.app`

| Path | Contents |
|---|---|
| `Contents/MacOS/Lotus` | Universal Swift menu-bar binary (~1 MB per slice) |
| `Contents/MacOS/Lotus_Lotus.bundle` | SPM resource bundle (bot script, Python package, lockfile) |
| `Contents/Resources/bin/uv` | Universal `uv` binary used by the installer + `launchd` service |
| `Contents/Resources/runtime-template/` | `bot_service.py`, `pyproject.toml`, `uv.lock`, `mac_mcp/` source |
| `Contents/Resources/assets/` | Logos, banner art, wizard images |
| `Contents/Resources/AppIcon.icns` | macOS app icon |

#### What gets created on first launch

| Path | Contents |
|---|---|
| `~/Library/Application Support/Lotus/runtime/` | Writable copy of the runtime template |
| `~/Library/Application Support/Lotus/runtime/.venv/` | Python 3.13 venv with all bot deps |
| `~/Library/Application Support/Lotus/config.json` | Bot credentials (token, allowed IDs, model) |
| `~/Library/Application Support/Lotus/logs/bot_service.log` | Runtime logs |
| `~/Library/Application Support/Lotus/control.port` | Port the local control API listens on |
| `~/Library/Application Support/Lotus/lotus_bot.pid` | PID file for the running bot |
| `~/Library/LaunchAgents/com.lotus.botservice.plist` | `launchd` plist that keeps the bot alive |

#### Menu-bar controls

Click the 🌸 icon:
- **Show Lotus** — opens the control panel window
- **Toggle Bot** — start/stop the `launchd` service
- **Quit Lotus** — quits the menu-bar app (the bot service keeps running independently)

Closing the window hides it; the app stays in the menu bar.

#### Uninstalling

```bash
# Stop and unregister the launchd service
launchctl bootout "gui/$(id -u)/com.lotus.botservice"

# Remove all Lotus state
rm -rf ~/Library/Application\ Support/Lotus
rm    ~/Library/LaunchAgents/com.lotus.botservice.plist

# Drag /Applications/Lotus.app to the Trash
```

**Source:** [`Mac-MCP/`](Mac-MCP) · **Lead:** [@JayashBhandary](https://github.com/JayashBhandary) · **Setup guide:** [`Mac-MCP/SETUP.md`](Mac-MCP/SETUP.md)

---

## 🏗️ Architecture

Lotus is a three-tier system on both platforms.

```
┌────────────────────────────────────────────────────────┐
│                       Telegram                         │  ← user
└──────────────────────────┬─────────────────────────────┘
                           │  long-poll updates
┌──────────────────────────▼─────────────────────────────┐
│            bot_service.py (background)                 │
│                                                        │
│  ┌──────────────────┐   ┌────────────────────────┐    │
│  │ telegram_bot     │   │ control_api (HTTP)     │    │
│  │  - command parse │   │  - GET /api/status     │    │
│  │  - priority chain│   │  - GET /api/logs       │    │
│  └────────┬─────────┘   │  - POST /api/restart   │    │
│           │             └────────────────────────┘    │
│  ┌────────▼─────────────────────────────────────┐     │
│  │ MCP-style tool surface (mac_mcp / win_mcp)   │     │
│  │  - desktop (mouse, keyboard, screenshot)     │     │
│  │  - filesystem (find, open, ls, cd)           │     │
│  │  - media (yt-dlp, ffmpeg, mpv)               │     │
│  │  - research (wiki, DDG, scrape, PDF)         │     │
│  │  - tts / voice                               │     │
│  └────────┬─────────────────────────────────────┘     │
└───────────┼───────────────────────────────────────────┘
            │
┌───────────▼────────────┐    ┌──────────────────────┐
│      Ollama daemon     │    │    Native GUI        │
│   (local LLM, http)    │    │ Lotus.app / Tray     │
│                        │    │ (status + control)   │
└────────────────────────┘    └──────────────────────┘
```

- **Telegram bot** — `python-telegram-bot`, long-polling, gated by an
  allowlist of user IDs. Anything from outside the list is dropped.
- **MCP server** — `fastmcp`-based tool surface that's exposed to both
  the bot loop and (optionally) external MCP clients like Claude Desktop.
- **Control API** — a tiny `uvicorn` HTTP server on `localhost:40510`,
  used by the GUI to query status and trigger restarts. Bound to
  loopback only.
- **GUI** — a thin client over the control API. The bot service is
  authoritative; the GUI never owns state.
- **Ollama** — out-of-process local LLM server. Lotus speaks to it over
  HTTP at `http://127.0.0.1:11434`.

### Process supervision

| Platform | Mechanism |
|---|---|
| **Windows** | Scheduled task at logon, run hidden via `pythonw.exe` |
| **macOS**   | `launchd` user agent (`com.lotus.botservice.plist`), `RunAtLoad=true`, `KeepAlive=false` |

`KeepAlive=false` is intentional — if the service crashes hard we don't
want a tight respawn loop. Use the GUI's **Restart** button or
`launchctl kickstart -k …` to bring it back.

### Why Telegram?

- **Universal** — the same client works on iOS, Android, web, and desktop.
- **Free message API** — no SMS / Twilio dependencies.
- **Bot tokens are revocable** — if a token leaks you regenerate it via BotFather.
- **End-to-end optional** — Lotus uses standard bot API, but you can layer
  a private channel or [Telegram MTProxy](https://core.telegram.org/mtproto/mtproto-transports/intermediate)
  if you want extra hop secrecy.

---

## ⚙️ Configuration

### Bot config (`config.json`)

| Field | Type | Description |
|---|---|---|
| `name` | string | Display name (used in greetings: "Hello \<name\>") |
| `telegram_token` | string | Bot token from BotFather |
| `allowed_user_id` | string | Comma-separated list of Telegram user IDs allowed to issue commands |
| `model_name` | string | Ollama model identifier — must already be `ollama pull`'d |
| `created_at` | string | ISO-8601 timestamp written by the wizard |

Example:

```json
{
  "name": "Jayash",
  "telegram_token": "1234567890:ABC-defGhIjKlmNoPqRStUvWxYz1234567890",
  "allowed_user_id": "1327255784,9876543210",
  "model_name": "qwen2.5:3b",
  "created_at": "2026-05-09 00:33:21"
}
```

| Platform | Path |
|---|---|
| Windows | `%LOCALAPPDATA%\Lotus\config.json` |
| macOS   | `~/Library/Application Support/Lotus/config.json` |

### Environment variables

| Variable | Default | Effect |
|---|---|---|
| `LOTUS_CONTROL_PORT` | `40510` | Port the local control API listens on |
| `MAC_MCP_SCREENSHOT_BACKEND` | `auto` | `auto`, `quartz`, or `screencapture` (macOS only) |
| `MAC_MCP_DEBUG` | `false` | Verbose MCP server logging |
| `ANONYMIZED_TELEMETRY` | `true` | Set to `false` to disable PostHog event reporting |

On macOS these can be added to the `EnvironmentVariables` block of the
`launchd` plist if you want them to apply to the auto-started service.

### Control API (localhost-only)

```bash
PORT=$(cat ~/Library/Application\ Support/Lotus/control.port 2>/dev/null || echo 40510)

curl http://127.0.0.1:$PORT/api/status     # service health + uptime
curl http://127.0.0.1:$PORT/api/logs       # last 100 log lines
curl http://127.0.0.1:$PORT/api/config     # current config (token redacted)
curl -X POST http://127.0.0.1:$PORT/api/restart
curl -X POST http://127.0.0.1:$PORT/api/stop
```

The Swift menu-bar app uses this same surface — there is no private API.

---

## 🔐 Privacy & Security

Lotus is designed to be **private by default**:

- ✅ **Local LLM only** — Ollama runs on your machine. Prompts and
  conversations never touch a third-party API.
- ✅ **Allowlist authentication** — Telegram user IDs not in
  `allowed_user_id` are silently ignored. The bot does not respond,
  log, or acknowledge them.
- ✅ **Loopback control API** — bound to `127.0.0.1` only; not exposed
  on any network interface.
- ✅ **No outbound telemetry by default** for the macOS app's installer
  steps — set `ANONYMIZED_TELEMETRY=false` in `.env` to also disable
  the bot's optional PostHog events.
- ✅ **Token storage** — `config.json` is mode `0600` after the wizard
  writes it. The macOS Swift GUI redacts tokens in the **Settings**
  view.

### Threat model (briefly)

| Concern | Mitigation |
|---|---|
| Bot token leaks | Revoke via BotFather, regenerate, rewrite `config.json` |
| Allowed user phone gets compromised | Remove their ID from `allowed_user_id`, restart |
| Local code execution by a permitted user | Lotus *is* a remote-control agent; trust the allowlist accordingly |
| Network sniffer on home wifi | Telegram traffic is TLS; control API is loopback |
| Malicious DMG / EXE | Verify the SHA-256 from the Release page against `SHA256SUMS.txt` |

> **The macOS DMG is ad-hoc signed but not notarized.** Verify the
> SHA-256 against the published checksums file before installing if
> you want strong tamper-evidence.

---

## 🩺 Troubleshooting

### macOS: "Lotus.app is damaged and can't be opened"
Gatekeeper blocked the unsigned bundle. Right-click → **Open** once, or:
```bash
xattr -d com.apple.quarantine /Applications/Lotus.app
```

### macOS: bot won't start, "plist not installed"
The first-run wizard didn't complete. Open Lotus and re-run the
installer; or install the `launchd` agent manually:
```bash
bash Mac-MCP/install_scripts/install.sh
```

### macOS: control panel is blank / app crashes on launch
You're on a v2.0.0 build. Upgrade to **v2.0.1** (it ships the SPM
resource bundle that v2.0.0 was missing).

### Either platform: bot is silent in Telegram
1. Confirm the bot is running:
   ```bash
   # macOS
   launchctl print gui/$(id -u)/com.lotus.botservice | head -20
   # Windows
   Get-ScheduledTask -TaskName "Lotus*"
   ```
2. Confirm your user ID is in `allowed_user_id`. Get yours from
   [@userinfobot](https://t.me/userinfobot).
3. Tail the logs:
   ```bash
   tail -f ~/Library/Application\ Support/Lotus/logs/bot_service.log     # macOS
   Get-Content "$env:LOCALAPPDATA\Lotus\logs\bot_service.log" -Wait      # Windows
   ```

### Ollama is unreachable / `ollama_reachable: false`
1. Start Ollama: `ollama serve`.
2. Pull the configured model: `ollama pull qwen2.5:3b`.
3. The control panel's status row goes green within ~5 seconds.

### Music plays nothing on macOS
ffmpeg + mpv must be installed. The first-run wizard offers to install
them via Homebrew (optional step). To do it manually:
```bash
brew install mpv ffmpeg
```

### "Port 40510 already in use"
Set a different port for the launchd / scheduled task:
```bash
# macOS — edit the plist's EnvironmentVariables section
plutil -replace EnvironmentVariables.LOTUS_CONTROL_PORT \
  -string "40520" \
  ~/Library/LaunchAgents/com.lotus.botservice.plist
launchctl kickstart -k gui/$(id -u)/com.lotus.botservice
```

---

## 🛠️ Development

### Prerequisites

| Need | Install |
|---|---|
| Python 3.13 | `brew install python@3.13` (mac) or [python.org](https://www.python.org/) (win) |
| `uv` | `curl -LsSf https://astral.sh/uv/install.sh \| sh` |
| Xcode 16 (Swift 6) | Mac App Store — required for `Mac-MCP/ControlPanel/` |
| Visual Studio Build Tools | required to compile native deps on Windows |
| Ollama | `brew install ollama` or [ollama.com](https://ollama.com) |

### macOS — building from source

```bash
git clone https://github.com/SatyamPote/Lotus.git
cd Lotus/Mac-MCP

# Python deps
uv sync

# Swift app — universal arm64 + x86_64
bash ControlPanel/make_app.sh

# DMG installer
bash ControlPanel/make_dmg.sh

# Run the built app
open Lotus.app
```

For an **arm64-only build** (faster, useful during dev on Apple Silicon):
```bash
LOTUS_ARCHS="arm64" bash ControlPanel/make_app.sh
```

### macOS — running the Swift app from Xcode

```bash
cd Mac-MCP/ControlPanel
swift build -c debug
swift run Lotus
```

The dev build walks up parent directories to find `bot_service.py`, so
you can edit Python and Swift in parallel without rebuilding the bundle.

### Windows — building from source
See [`Windows-MCP/`](Windows-MCP) for the Inno Setup project, the
PyInstaller spec, and the build instructions there.

### Running tests
```bash
cd Mac-MCP
uv run pytest                      # Python bot + tools
swift test --package-path ControlPanel  # Swift app (when added)
```

### Code style
- **Python:** `ruff` (config in `pyproject.toml`); `pyrefly` for type checking
- **Swift:** standard `swift-format` with project defaults
- **Commit messages:** Conventional Commits aren't enforced, but
  short imperative subjects ("add X", "fix Y", "release notes: vZ.Z.Z")
  keep the auto-generated GitHub Release notes readable.

---

## 🏷️ Releases & CI/CD

The macOS pipeline is fully automated via GitHub Actions
([`.github/workflows/swift.yml`](.github/workflows/swift.yml)).

### Release triggers

| Trigger | What runs | What ships |
|---|---|---|
| Push tag `v*` (e.g. `v2.0.1`) | Build + publish | DMG attached to the GitHub Release |
| Manual `workflow_dispatch` (publish=false) | Build only | Workflow artifact |
| Manual `workflow_dispatch` (publish=true + version) | Build + tag + publish | DMG attached to the GitHub Release |

The workflow:

1. Pins **Xcode 16 / Swift 6** on `macos-15` runners.
2. Runs `uv sync` to populate the venv.
3. Stamps the resolved version into `make_app.sh` and `make_dmg.sh`.
4. Builds the **universal** `Lotus.app`.
5. **Verifies** the binary contains both `arm64` and `x86_64` slices
   (`lipo -archs`) — fails the build otherwise.
6. Builds the **DMG** with the `dmg_banner.png` background.
7. **Verifies** the DMG mounts cleanly with `Lotus.app` + `Applications`
   inside.
8. On a tagged or manual-publish run: creates a GitHub Release with the
   DMG, `SHA256SUMS.txt`, and curated release notes from
   [`release-notes/v<version>.md`](release-notes), with auto-generated
   PR notes appended underneath.

See [`.github/RELEASING.md`](.github/RELEASING.md) for the full pipeline
runbook.

### Recent macOS releases

| Version | Date | Highlights |
|---|---|---|
| [v2.0.1](release-notes/v2.0.1.md) | 2026-05-09 | Patch — fix launch crash from missing SPM resource bundle. |
| [v2.0.0](release-notes/v2.0.0.md) | 2026-05-09 | Truly standalone install — bundled `uv` + runtime template. Universal binary. |
| [v1.0.0](release-notes/v1.0.0.md) | 2026-05-04 | First public macOS release. |

### Cutting a new release

```bash
# 1. Write curated release notes
cp release-notes/TEMPLATE.md release-notes/v2.1.0.md
$EDITOR release-notes/v2.1.0.md
git add release-notes/v2.1.0.md
git commit -m "release notes: v2.1.0"

# 2. Tag and push — fires the workflow
git tag -a v2.1.0 -m "Lotus v2.1.0"
git push origin v2.1.0
```

---

## 📁 Repository Layout

```
Lotus/
├── README.md                      ← you are here
├── RELEASE.md                     # legacy release pointer
├── pyrefly.toml                   # type-checker config
│
├── Windows-MCP/                   # Windows AI agent
│   ├── ...                        # PyInstaller spec, Inno Setup project
│   └── ...                        # tray app, command engine, voice, research
│
├── Mac-MCP/                       # macOS native menu-bar app + bot
│   ├── ControlPanel/              # Swift Package — Lotus.app source
│   │   ├── Package.swift
│   │   ├── make_app.sh            # builds Lotus.app
│   │   ├── make_dmg.sh            # builds the DMG
│   │   └── Sources/Lotus/
│   │       ├── LotusApp.swift
│   │       ├── AppDelegate.swift
│   │       ├── AppState.swift
│   │       ├── Models/
│   │       ├── Services/
│   │       └── Views/
│   ├── src/mac_mcp/               # Python MCP server + Telegram bot
│   ├── bot_service.py             # bot service entry point
│   ├── pyproject.toml
│   ├── uv.lock
│   ├── install_scripts/           # launchd plist installer
│   ├── assets/                    # logos, banner art, wizard images
│   ├── tests/                     # pytest suite
│   └── SETUP.md
│
├── release-notes/                 # per-release curated notes
│   ├── README.md
│   ├── TEMPLATE.md
│   ├── v1.0.0.md
│   ├── v2.0.0.md
│   └── v2.0.1.md
│
└── .github/
    ├── workflows/swift.yml        # macOS build & release pipeline
    └── RELEASING.md               # pipeline runbook
```

---

## 🗺️ Roadmap

Tracked items, roughly prioritized.

### macOS
- [ ] **Apple notarization** — sign with a Developer ID cert, submit via
  `notarytool`, staple. Removes the Gatekeeper warning entirely.
- [ ] **Sparkle auto-updates** — generate `appcast.xml` in the workflow,
  point the app at it. One-click in-app updates.
- [ ] **Pre-baked `.DS_Store`** — fully styled DMG window from CI
  (currently CI skips Finder styling on headless runners).
- [ ] **Code-sign the bundled `uv`** so the whole app re-signs cleanly
  after notarization.

### Windows
- [ ] **MSIX installer** — modern packaging alongside the Inno Setup EXE.
- [ ] **Code-signing certificate** — eliminate SmartScreen warnings.

### Cross-platform
- [ ] **Multi-user mode** — per-user runtime sandboxes within a single install.
- [ ] **Plugin system** — third-party MCP tools can be dropped into a
  `~/.lotus/plugins/` dir and hot-loaded.
- [ ] **Mobile-first dashboard** — render the control panel as a TWA the
  bot can DM you a link to.
- [ ] **Offline TTS quality** — investigate Piper / Coqui as the local
  voice engine.

---

## 👥 Contributors

Lotus is the product of two leads, one on each platform:

<table>
<tr>
<td align="center">
<a href="https://github.com/SatyamPote">
<img src="https://github.com/SatyamPote.png" width="120" alt="Satyam Pote"><br>
<b>Satyam Pote</b>
</a><br>
<sub>Project creator · Windows lead</sub><br>
<sub><a href="https://github.com/SatyamPote">@SatyamPote</a></sub><br>
<br>
<i>Designed and built the original Lotus agent, the Windows tray app,
the priority routing engine, the multi-source research pipeline, and
the Inno Setup deployment story.</i>
</td>
<td align="center">
<a href="https://github.com/JayashBhandary">
<img src="https://github.com/JayashBhandary.png" width="120" alt="Jayash Bhandary"><br>
<b>Jayash Bhandary</b>
</a><br>
<sub>macOS lead</sub><br>
<sub><a href="https://github.com/JayashBhandary">@JayashBhandary</a></sub><br>
<br>
<i>Designed and built the macOS native menu-bar app (`Lotus.app`),
the universal-binary build pipeline, the standalone DMG installer
with bundled `uv` runtime, the writable runtime-dir architecture,
and the GitHub Actions release workflow.</i>
</td>
</tr>
</table>

### Contributing

Pull requests and issues are welcome. Please:

1. Open an issue first for anything non-trivial — saves you from
   building something we'd want differently.
2. For UI work, include a screenshot or short screen recording.
3. For new MCP tools, add a docstring describing the user-facing
   command, expected arguments, and what state it touches.
4. Keep curated release notes in [`release-notes/`](release-notes) up
   to date — they ship as the GitHub Release body.

---

## 📜 License

This project is released under the MIT License — see
[`LICENSE`](LICENSE) for the full text.

The bundled `uv` binary is distributed under the [MIT/Apache-2.0
license](https://github.com/astral-sh/uv) by Astral. Ollama models you
pull are subject to their respective upstream licenses.

---

<div align="center">

**Built for stability. Built for privacy. Built for both Windows and Mac.**

🌸

</div>
