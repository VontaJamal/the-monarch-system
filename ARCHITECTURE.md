# Architecture — Shadowgate

## Overview

Shadowgate is a gamified quest engine that runs 24/7 on a Windows server, orchestrated by an AI agent (Raya/OpenClaw). The codebase lives in a GitHub repo, developed on Mac, but the live runtime is Windows.

## Environments

### Development (Mac)
- **Purpose:** Write code, test locally, push to GitHub
- **User:** `vonta@Mac.lan`
- **Repo location:** `~/Documents/Code Repos/the-system/` (or wherever you clone it)
- **Node/Python/whatever runtime:** local dev only
- **Git remote:** GitHub (source of truth for CODE)

### Production (Windows Server)
- **Purpose:** 24/7 runtime — quest delivery, state tracking, XP management
- **Host:** `your-server` (YOUR-HOST)
- **User:** `devon` (runs as Administrator)
- **SSH:** `ssh -i ~/.ssh/id_ed25519 user@your-server`
- **Repo clone location:** `D:\Code Repos\the-system\` (or similar)
- **State file:** `~\.openclaw\workspace\memory\leveling.json` (the LIVE player state)
- **Always running.** This machine never sleeps. It's Raya's body.

### Sync Flow
```
GitHub Repo (source of truth for code)
    ↓ git pull (on Windows, automated or manual)
    ↓
Windows Server (production runtime)
    → Reads/writes leveling.json (live state)
    → Delivers quests via Telegram/OpenClaw
    → Tracks completions, XP, streaks
    ↓ workspace sync (Windows → Mac, every 60s)
    ↓
Mac (gets state updates for visibility, but doesn't run anything)
```

## Key Separation: Code vs State

| What | Where | Why |
|------|-------|-----|
| Source code | GitHub repo | Version controlled, developed on Mac |
| `leveling.json` (live state) | Windows: `~\.openclaw\workspace\memory\leveling.json` | This is the LIVE player state. XP, streaks, quest history. Windows is source of truth. |
| Config/templates | GitHub repo | Quest templates, rank definitions, punishment lists — part of the codebase |
| Runtime logs | Windows | Quest delivery logs, error logs, audit trail |

**IMPORTANT:** The code should NOT bundle a `leveling.json` with real player data. The repo should include a `leveling.template.json` or `leveling.example.json` for new users. The live state file lives OUTSIDE the repo on the Windows server.

## Runtime Integration

### How It Runs Today (v0 — OpenClaw Cron)
Currently Shadowgate runs as an OpenClaw cron job:
- **Cron ID:** `c1506444-9235-4bc9-b951-9e124b0dc26e`
- **Schedule:** 6:00 AM ET daily
- **Type:** Isolated `agentTurn` — spins up a Sonnet agent that reads `leveling.json`, generates quests, delivers to Telegram
- **Delivery:** Telegram group `YOUR_CHAT_ID` (Raya HQ)

### Target Architecture (v1 — Standalone Engine)
The goal is to replace the cron-driven AI generation with actual code:

```
┌─────────────────────────────────────────────┐
│           Windows Server (24/7)              │
│                                              │
│  ┌──────────────┐    ┌───────────────────┐  │
│  │  Shadowgate  │    │   leveling.json   │  │
│  │  (Node/Python)│───→│   (live state)    │  │
│  │               │    └───────────────────┘  │
│  │  - Quest Gen  │                           │
│  │  - XP Calc    │    ┌───────────────────┐  │
│  │  - Neglect    │───→│   Telegram API    │  │
│  │  - Streaks    │    │   (quest delivery)│  │
│  │  - Punishments│    └───────────────────┘  │
│  │  - Weekly     │                           │
│  │    Tracking   │    ┌───────────────────┐  │
│  │               │───→│   Git Integration │  │
│  │               │    │   (auto-detect    │  │
│  └──────────────┘    │    completions)   │  │
│                       └───────────────────┘  │
│                                              │
│  Runs as: NSSM service / scheduled task /    │
│  OpenClaw cron trigger                       │
└─────────────────────────────────────────────┘
```

### Deployment Flow
```bash
# On Mac (development):
git add . && git commit -m "feat: add streak bonus logic" && git push

# On Windows (production — manual or automated):
cd D:\Code Repos\the-system
git pull origin main
# Service auto-restarts or picks up changes on next cycle
```

Could also set up a GitHub webhook or polling script on Windows to auto-pull on push.

## File Structure (Proposed)

```
the-system/
├── README.md                    # Full spec and documentation
├── ARCHITECTURE.md              # This file
├── package.json / pyproject.toml
├── src/
│   ├── engine.ts                # Core quest engine
│   ├── quest-generator.ts       # Quest generation logic
│   ├── xp-calculator.ts         # XP and leveling math
│   ├── neglect-tracker.ts       # Project neglect monitoring
│   ├── punishment-engine.ts     # Punishment selection and enforcement
│   ├── public-output-tracker.ts # Weekly output deadline tracking
│   ├── streak-manager.ts        # Streak calculation and bonuses
│   ├── delivery/
│   │   ├── telegram.ts          # Telegram delivery
│   │   ├── discord.ts           # Discord delivery (future)
│   │   └── formatter.ts         # System notification formatting
│   ├── integrations/
│   │   ├── git-monitor.ts       # Watch repos for commits (auto-complete quests)
│   │   ├── social-scraper.ts    # Check X/LinkedIn for public outputs (future)
│   │   └── openclaw.ts          # OpenClaw integration hooks
│   └── types.ts                 # TypeScript types / schemas
├── config/
│   ├── projects.json            # Project definitions (checked into repo)
│   ├── ranks.json               # Rank progression table
│   ├── punishments.json         # Punishment templates
│   └── quest-templates.json     # Quest idea templates per project/topic
├── leveling.example.json        # Example state file (for new users)
├── tests/
│   └── ...
└── scripts/
    ├── install-windows.ps1      # Set up as NSSM service on Windows
    └── deploy.ps1               # Pull latest and restart service
```

## Windows-Specific Considerations

### Service Management
- Run as an **NSSM service** (same pattern as CryptoPipeline, OptionsBot, etc.)
- Service name: `TheSystem` or `SoloLevelingSystem`
- Auto-restart on crash
- Logs to `D:\Code Repos\the-system\logs\`

### File Paths
- State file: `~\.openclaw\workspace\memory\leveling.json`
- The code should accept a `--state-path` flag or `STATE_PATH` env var so it's not hardcoded
- Default can point to the OpenClaw workspace path

### Timezone
- All times in **Eastern (America/New_York)**
- Quest window: 7 AM – 11 PM ET
- Daily quest drop: 6 AM ET
- Weekly deadline: Sunday 11 PM ET

### Git Monitoring
- Repos are on `D:\Code Repos\` and `~\.openclaw\workspace\`
- Git monitor should watch these paths for new commits
- On commit detected → check if it satisfies any open quest → auto-complete if match

## Environment Variables

```env
# Required
STATE_PATH=~\.openclaw\workspace\memory\leveling.json
TELEGRAM_BOT_TOKEN=<from OpenClaw config or separate>
TELEGRAM_CHAT_ID=YOUR_CHAT_ID

# Optional
TIMEZONE=America/New_York
QUEST_DROP_HOUR=6
LOG_PATH=D:\Code Repos\the-system\logs
GIT_REPOS_PATH=D:\Code Repos
```

## API (Future)

If we want a dashboard or interactive quest completion:

```
GET  /player          → Current player stats
GET  /quests/today    → Today's issued quests
POST /quests/:id/complete  → Mark quest complete (with proof link for [PUBLIC])
GET  /history         → Quest history
GET  /streak          → Current streak info
POST /weekly-output   → Submit weekly public output link
```

Dashboard could run on the same Windows server (port 8421 or similar, next to the trading dashboard on 8420).

