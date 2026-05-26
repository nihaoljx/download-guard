# Download Guard

> **AI Agent download guard for Windows.** Transparent locations, no C-drive fill-up, auto log cleanup.

[![Platform: Windows](https://img.shields.io/badge/platform-Windows-blue)](https://github.com)
[![License: MIT-0](https://img.shields.io/badge/license-MIT--0-green)](LICENSE)
[![Version: 5.4.0](https://img.shields.io/badge/version-5.4.0-orange)](CHANGELOG.md)

---

## The Problem

When you give an AI coding agent (Claude Code, Cursor, etc.) permission to install packages and download files, you lose visibility into **where things go**. Common pain points:

- 🤷 **"Where did it download?"** — Agent silently installs to C: drive, you have no idea
- 💥 **C: drive fills up** — Large models (2-14 GB each), npm global packages, pip caches all pile up on C:
- 🔄 **Repeated downloads** — Agent re-downloads the same model/dataset because it forgot where it put it
- 🗑️ **Invisible garbage** — Log files, caches, and temp files accumulate forever with no cleanup
- ⚠️ **Broken PATH** — After moving npm prefix or conda envs, commands stop working
- 💿 **Drive disconnected** — Your download directory is on an external drive that got unplugged, Agent silently falls back to C:

**Download Guard fixes all of this.**

---

## What It Does For You

| Problem | How Download Guard Helps |
|---------|------------------------|
| Don't know where files went | **Every download tells you exactly where it went** — filename, path, disk health status |
| C: drive fills up | **Downloads go to your configured directory** (not C:), with space checks before every download |
| Drive disconnected or path broken | **BLOCKS the download instead of silently falling back to C:** — you get a clear error message |
| Repeated downloads | **Detects if the file already exists** in your download directory and asks before re-downloading |
| Garbage accumulation | **Auto-archives old log entries** after 30 days, deletes oversized archives |
| Broken PATH after migration | **Checks PATH alignment** when changing npm prefix/conda envs, warns about spaces in tool paths |
| Tool caches on C: | **Scans npm/pip/conda/cargo/go/uv/pnpm caches** and offers one-click migration |
| Large downloads surprise | **Warns if download size > 50% of free space**, blocks if it exceeds free space |
| venv vs global pip | **Recognizes active virtual environments** — `pip install` inside venv doesn't trigger |

---

## How It Works — The Flow

### When you trigger a download (or the Agent does)

```
┌─────────────────────────────────────────────────────────────────────┐
│                    Download Guard Flow                               │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  1. READ CONFIG                                                     │
│     └─ Read config.md → Get DOWNLOAD_ROOT                           │
│     └─ If SETUP_DONE = false → Run first-time setup                │
│     └─ Verify config integrity                                      │
│                                                                      │
│  2. CLASSIFY PATH TYPE                                              │
│     └─ Type A (cache) → Safe to redirect (pip cache, npm cache)   │
│     └─ Type B (install dir) → Must sync PATH (npm prefix, cargo)  │
│     └─ Type C (file storage) → Safe to redirect (git clone, wget)  │
│                                                                      │
│  3. DUPLICATE CHECK                                                 │
│     └─ Scan DOWNLOAD_ROOT for similar files                         │
│     └─ If found → Ask user: "Already exists. Download again?"      │
│                                                                      │
│  4. PATH AVAILABILITY + SPACE CHECK                                 │
│     └─ Drive exists? → No → BLOCK (exit 2, no C: fallback)        │
│     └─ Path writable? → No → BLOCK                                 │
│     └─ Path missing? → Auto-create if drive is valid               │
│     └─ Space check → Below minimum? → BLOCK                        │
│     └─ Size awareness → > 50% free space? → WARN                   │
│                                                                      │
│  5. INFORM USER                                                     │
│     └─ "准备下载: {file} → {path} | Disk: {X}GB free [OK]"       │
│                                                                      │
│  6. EXECUTE DOWNLOAD                                                │
│                                                                      │
│  7. VERIFY + LOG + INFORM                                           │
│     └─ Verify file exists at expected path                          │
│     └─ Log to download-log.md (with auto-cleanup)                   │
│     └─ "完成: {file} → {path}"                                     │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

### Daily first trigger — environment report

The first time each day the skill activates, it shows an environment report:

```
[DOWNLOAD GUARD] Today's first check · Environment Report
Download dir  : D:\AI-Downloads  (132 GB available)
Path available: OK
C: drive      : 58 GB available  [OK]
Tool caches   : All OK / 2 items on C: [WARN]
Log entries   : 47 entries
```

If the download directory is unavailable → **immediately alerts you**.

---

## Quick Start

### Installation

**Option A: From ClawHub** (recommended)
```bash
clawhub install download-guard
```

**Option B: Manual**
1. Download/clone this repository
2. Copy the `download-guard` folder to `~/.workbuddy/skills/download-guard/`

### First-Time Setup

When the skill activates for the first time, it will automatically:

1. **Scan your disks** and recommend the best non-C: drive (largest free space)
2. **Ask you to confirm** the download directory
3. **Scan tool caches** on C: drive and offer to migrate them
4. **Show a status card** when ready

Example first-time prompt:
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  Download Guard · First-time Setup
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Disk scan results:

  F:  132 GB available  ← Recommended (most space)
  E:   97 GB available
  D:   39 GB available
  C:   58 GB available  [System drive - Not recommended]

Suggested: F:\AI-Downloads
Confirm? Or enter a different path:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Daily Usage

Once configured, every download automatically:

1. ✅ Checks disk space and path availability
2. ✅ Detects duplicate downloads
3. ✅ Shows you where the file is going
4. ✅ Verifies the file landed correctly
5. ✅ Logs the download with auto-cleanup

**You don't need to do anything.** The guard runs automatically when:
- `pip install`, `npm install -g`, `cargo install`, `go install`, `uv pip install`
- `pnpm add -g`, `bun install -g`, `conda install`
- `git clone`, `docker pull`, `ollama pull`, `huggingface-cli download`
- `curl`, `wget`, `winget install`, `choco install`, `scoop install`

---

## Commands

Say any of these to your AI agent:

| Command | What Happens |
|---------|-------------|
| `"download log"` / `"下载日志"` | Show last 20 downloads |
| `"scan cache"` / `"扫描缓存"` | Scan tool cache locations |
| `"migrate cache"` / `"迁移缓存"` | Migrate C: drive caches to your download root |
| `"disk space"` / `"磁盘空间"` | Check disk space |
| `"where's my download"` / `"刚才下的在哪"` | Show last download location |
| `"check path"` / `"检查路径"` | Verify download directory is healthy |
| `"download guard version"` / `"下载版本"` | Show current version |
| `"reset config"` / `"重置配置"` | Re-run first-time setup |
| `"fix warnings"` / `"帮我修复"` | Auto-fix all warnings |
| `"uninstall download guard"` | Show cleanup instructions |

---

## Supported Package Managers & Tools

| Tool | Trigger | What's Guarded |
|------|---------|---------------|
| pip | `pip install` | Cache location, site-packages |
| uv | `uv pip install` | Cache location |
| npm | `npm install -g` | Prefix + cache + PATH |
| pnpm | `pnpm add -g` | Store location |
| bun | `bun install -g` | Install location |
| conda | `conda install` | Package cache + env dirs |
| yarn | `yarn global add` | Cache location |
| cargo | `cargo install` | CARGO_HOME + registry |
| go | `go install` | GOPATH + module cache |
| git | `git clone` | Clone destination |
| Docker | `docker pull` | Image download |
| Ollama | `ollama pull` | Model download |
| HuggingFace | `huggingface-cli download` | Model/dataset download |
| winget/choco/scoop | `winget install` etc. | Package download |
| curl/wget | `curl` / `wget` | File download |

> **Smart detection**: `pip install` inside an active virtual environment (venv) does NOT trigger the guard — venv installs are local and don't affect C: drive.

---

## Configuration

Single file: `config.md` in the skill directory. Edit by hand or let the Agent manage it.

| Parameter | Default | Meaning |
|-----------|---------|---------|
| `DOWNLOAD_ROOT` | *(set during setup)* | Where downloads go |
| `SETUP_DONE` | `false` | Whether setup has been completed |
| `MIN_FREE_GB` | `0.5` | Below this = BLOCK download |
| `WARN_FREE_GB` | `2` | Below this = WARN but proceed |
| `C_DRIVE_WARN_GB` | `5` | C: below this = extra warning |
| `ALLOW_SPACE_IN_TOOL_PATH` | `false` | Allow spaces in npm/conda paths (dangerous) |
| `EXEMPT_PATHS` | *(empty)* | Paths NOT guarded by this skill |
| `LOG_RETENTION_DAYS` | `30` | Auto-archive entries older than this |
| `LOG_ARCHIVE_MAX_MB` | `10` | Delete archives larger than this |

> Set `LOG_RETENTION_DAYS: 0` to disable log cleanup entirely.

---

## File Structure

```
download-guard/
├── SKILL.md              # Core rules (loaded when skill triggers)
├── reference.md           # Detailed reference (loaded on demand)
├── config.md              # User configuration (template: SETUP_DONE=false)
├── CHANGELOG.md           # Version history
├── LICENSE                 # MIT-0
├── README.md               # This file
└── scripts/
    ├── check-space.ps1    # Disk space + path availability check
    ├── log-download.ps1   # Download logging + auto cleanup
    ├── scan-tool-cache.ps1 # Tool cache location scanner
    └── migrate-cache.ps1  # One-click cache migration
```

---

## Design Principles

1. **Transparent, not silent** — Every download tells you where it goes and if your disk is healthy
2. **Block, don't fallback** — If the configured path is unavailable, BLOCK rather than silently use C:
3. **Rules, not questions** — Ask on first setup and environment changes; daily ops = inform only
4. **Auto-clean, not accumulate** — Logs auto-archive; nothing grows forever
5. **Smart, not noisy** — venv installs don't trigger; small files don't trigger; duplicate downloads get flagged

---

## Known Limitations

| Limitation | Details |
|-----------|---------|
| Windows only | PowerShell scripts; macOS/Linux not supported |
| nvm-managed Node.js | nvm version switches may reset npm prefix to C: |
| `pip install --user` | Installs to C: user site-packages — suggest using venv instead |
| Docker image storage | `docker pull` images are managed by Docker Desktop (WSL2 VHDX) |
| Build artifacts | `cmake build`, `cargo build` outputs not in scope |
| System downloads | Windows Update / Defender not in scope |

---

## Requirements

- **Platform**: Windows (PowerShell 5.1+)
- **Disk space**: At least 0.5 GB free on target drive
- **Agent**: WorkBuddy / Claude Code with skill support

---

## License

MIT-0 — do whatever you want with it. See [LICENSE](LICENSE).
