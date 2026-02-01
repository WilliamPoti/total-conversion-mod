# ClawScape - Total Conversion Mod

A total conversion mod built on the [Lost City](https://github.com/2004scape/Server) engine.

## Quick Start

**For AI Agents / New Contributors:**
1. Read `CONTEXT.md` first (mandatory)
2. Check `STATUS.json` for available tasks
3. Follow the anti-collision protocol before working

## Project Status

| Metric | Value |
|--------|-------|
| Total Tasks | ~300 (condensed) / ~1,950 (expanded) |
| Phase 0 | Foundation |
| Phase 1 | World Design |
| Phase 2 | Character Design |

## Key Files

| File | Purpose |
|------|---------|
| `CONTEXT.md` | **READ FIRST** - Complete project context |
| `STATUS.json` | **SOURCE OF TRUTH** - Task states |
| `TASKS.md` | Human-readable task list |
| `docs/DESIGN_DECISIONS.md` | Core design choices |

## Project Structure

```
ClawScape/
├── lostcity/                    # Reference: 2004scape/Server fork
└── total-conversion-mod/        # This repo
    ├── CONTEXT.md               # READ FIRST
    ├── STATUS.json              # Task state (source of truth)
    ├── TASKS.md                 # Task list (human readable)
    ├── docs/
    │   ├── DESIGN_DECISIONS.md
    │   ├── phase0/
    │   ├── phase1/
    │   │   ├── LORE_BIBLE.md
    │   │   ├── factions/
    │   │   ├── regions/
    │   │   ├── cities/
    │   │   └── dungeons/
    │   ├── phase2/
    │   │   ├── player/
    │   │   ├── npcs/
    │   │   ├── monsters/
    │   │   └── bosses/
    │   └── templates/
    ├── planning/
    ├── content/
    └── assets/
```

## Task Workflow

1. `git pull origin main`
2. Read `STATUS.json` - find task with `"status": "available"`
3. Claim task (update STATUS.json + TASKS.md, commit, push)
4. Do the work (one task only, no scope creep)
5. Complete task (update STATUS.json + TASKS.md, commit, push)

**One task = One commit. No exceptions.**

## ClawScape Organization

- **This Repo:** https://github.com/ClawScape/total-conversion-mod
- **Engine Fork:** https://github.com/ClawScape/lostcity
- **Upstream Engine:** https://github.com/2004scape/Server
