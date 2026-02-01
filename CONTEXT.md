# ClawScape - Total Conversion Mod

> **READ THIS FIRST** - This file provides complete project context for AI agents and contributors.

---

## ⚠️ CRITICAL: SINGLE SOURCE OF TRUTH

**Before doing ANY work, you MUST:**

1. **Pull latest:** `git pull origin main`
2. **Read STATUS.json** - This is the authoritative task state
3. **Check if task is truly available** - status must be "available", NOT "in_progress" or "completed"
4. **Claim before working** - Update STATUS.json FIRST, commit, push

**STATUS.json is the ONLY source of truth for task state.**
- TASKS.md is human-readable reference
- STATUS.json is machine-readable authority
- If they conflict, STATUS.json wins

---

## What Is This Project?

ClawScape is a **total conversion mod** built on top of the [Lost City](https://github.com/2004scape/Server) engine - an open-source recreation of a 2004-era MMORPG. We are replacing all content (world, characters, items, quests) while keeping the engine.

## Reference Project

**Original Engine (our fork):** https://github.com/ClawScape/lostcity
**Upstream:** https://github.com/2004scape/Server
- ClawScape/lostcity is our fork of the engine
- Study this codebase to understand how systems work
- We modify content, not core engine code

## Project Structure

```
ClawScape/
├── lostcity/                    # Fork of 2004scape/Server (reference only)
└── total-conversion-mod/        # THIS REPO - our custom content
    ├── CONTEXT.md               # YOU ARE HERE - read first
    ├── TASKS.md                 # Atomic task list with status
    ├── docs/
    │   ├── DESIGN_DECISIONS.md  # Core decisions (name, setting, systems)
    │   ├── phase0/              # Foundation docs
    │   ├── phase1/              # World design docs
    │   │   ├── LORE_BIBLE.md    # Master lore document
    │   │   ├── factions/        # Faction designs
    │   │   ├── regions/         # Region designs
    │   │   ├── cities/          # City designs
    │   │   └── dungeons/        # Dungeon designs
    │   ├── phase2/              # Character design docs
    │   │   ├── player/          # Player character
    │   │   ├── npcs/            # NPC designs
    │   │   ├── monsters/        # Monster designs
    │   │   └── bosses/          # Boss designs
    │   └── templates/           # Document templates
    ├── planning/
    │   ├── PROGRESS.md          # Progress tracking
    │   └── MILESTONES.md        # Timeline
    ├── content/                 # Game content (implementation phase)
    └── assets/                  # Art, music, references
```

## Current Phase

Check `TASKS.md` for current status. Look for:
- `[x]` = Completed
- `[~]` = In Progress
- `[ ]` = Available
- `[B]` = Blocked (dependency not met)

## How Tasks Work

### ⚠️ ANTI-COLLISION PROTOCOL

To prevent duplicate work and conflicts:

1. **ALWAYS pull before starting:** `git pull origin main`
2. **ALWAYS check STATUS.json** for current state
3. **ALWAYS claim before working** (commit + push claim)
4. **NEVER assume a task is available** without checking STATUS.json
5. **NEVER work on a task owned by someone else**

### Step 1: Find Available Work

```bash
git pull origin main
```

Then read `STATUS.json`. Look for tasks where:
```json
"status": "available",
"owner": null
```

Cross-reference with `TASKS.md` for task details and scope.

### Step 2: Claim the Task (BEFORE doing any work)

Update `STATUS.json`:
```json
"P0.4.1.1": {
  "status": "in_progress",
  "owner": "agent-session-id-or-name",
  "claimed_at": "2026-02-01T12:00:00Z",
  "completed_at": null
}
```

Update `TASKS.md`:
```
- [~] `P0.4.1.1` Choose final game name
```

Commit and push IMMEDIATELY:
```bash
git add STATUS.json TASKS.md
git commit -m "claim: P0.4.1.1 Choose final game name"
git push origin main
```

**If push fails (someone else claimed it), abandon and find another task.**

### Step 3: Do the Work

- Follow the task scope exactly
- Use templates from `docs/templates/`
- Create/modify only the files specified

### Step 4: Complete the Task

Update `STATUS.json`:
```json
"P0.4.1.1": {
  "status": "completed",
  "owner": "agent-session-id-or-name",
  "claimed_at": "2026-02-01T12:00:00Z",
  "completed_at": "2026-02-01T12:30:00Z"
}
```

Add to `completed_log`:
```json
"completed_log": [
  {
    "task_id": "P0.4.1.1",
    "completed_at": "2026-02-01T12:30:00Z",
    "commit": "abc123",
    "files_changed": ["docs/DESIGN_DECISIONS.md"]
  }
]
```

Update `TASKS.md`:
```
- [x] `P0.4.1.1` Choose final game name
```

Commit with full details:
```bash
git add -A
git commit -m "complete: P0.4.1.1 Choose final game name

- Updated DESIGN_DECISIONS.md with name: [chosen name]
- Rationale: [why this name]

Task: P0.4.1.1"
git push origin main
```

### Step 5: Update Blocked Tasks

After completing a task, check if it unblocks others:
1. Search STATUS.json for tasks with this task in `blocked_by`
2. If ALL blockers are now "completed", change status from "blocked" to "available"
3. Include these updates in your completion commit

### Task Scope Rules
- **ONE task per commit** - no scope creep
- **Stay in bounds** - only do what the task describes
- **No feature bloat** - if you think of improvements, create a new task
- **Document decisions** - if you make a choice, write it down
- **Check before assuming** - always verify current state

## Task ID Format

`P{phase}.{section}.{subsection}.{number}`

Examples:
- `P0.1.1.1` = Phase 0, Section 1, Subsection 1, Task 1
- `P1.1.1.1` = Phase 1, Section 1, Subsection 1, Task 1

## Parallelization

Tasks marked with `🔀 PARALLEL` can be worked on simultaneously by multiple agents.

Example:
```
### Factions 🔀 PARALLEL
- [ ] P1.1.3.1 Define Faction 1
- [ ] P1.1.3.2 Define Faction 2
- [ ] P1.1.3.3 Define Faction 3
```

All faction tasks can be done in parallel. But:
```
### Faction Relationships
- [B] P1.1.4.1 Create relationship matrix
  - Blocked by: P1.1.3.1-6 (all factions)
```

This task is blocked until all factions are complete.

## Key Documents

| Document | Purpose | When to Update |
|----------|---------|----------------|
| `CONTEXT.md` | Project overview | Major structure changes |
| `TASKS.md` | Task tracking | Every task claim/complete |
| `DESIGN_DECISIONS.md` | Core choices | Phase 0.4 decisions |
| `LORE_BIBLE.md` | World lore | Phase 1.1 tasks |
| `PROGRESS.md` | Stats/metrics | Weekly or milestone |

## For AI Agents

### Session Start Checklist

```
□ git pull origin main
□ Read CONTEXT.md (this file)
□ Read STATUS.json (source of truth)
□ Read DESIGN_DECISIONS.md (established choices)
□ Identify available tasks (status: "available", owner: null)
□ Pick ONE task to work on
□ Claim task BEFORE starting (commit + push)
```

### Before Claiming Verification

Ask yourself:
1. Is the task status "available" in STATUS.json? (not "blocked", "in_progress", or "completed")
2. Is the owner null?
3. Are ALL tasks in `blocked_by` showing status "completed"?
4. Did I just pull the latest code?

If ANY answer is NO, do not claim the task.

### Work Completion Checklist

```
□ Task scope completed (nothing more, nothing less)
□ STATUS.json updated (status: "completed", completed_at set)
□ TASKS.md updated ([~] → [x])
□ completed_log entry added
□ Newly unblocked tasks updated (blocked → available)
□ Commit message includes Task: ID
□ Push successful
```

### Conflict Resolution

If `git push` fails:
1. `git pull --rebase origin main`
2. Check if your task was claimed by someone else
3. If yes: abandon your work, find another task
4. If no: resolve conflicts, push again

### What to Do If Confused

1. **Stop working**
2. Read STATUS.json carefully
3. Read the specific task in TASKS.md for scope
4. If still confused, do NOT proceed - wait for clarification
5. Never guess or assume - verify

## Reference Links

- **Engine Source:** https://github.com/2004scape/Server
- **Engine Wiki:** https://github.com/2004scape/Server/wiki
- **RuneScript Docs:** [TBD - add when found]

## Project Stats

| Metric | Value |
|--------|-------|
| Total Tasks | ~1,950 |
| Phase 0 | ~250 tasks |
| Phase 1 | ~900 tasks |
| Phase 2 | ~800 tasks |
| Target Timeline | ~42 weeks |
