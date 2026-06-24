# WFC3/IR Drizzling Pipeline
### Human Guide to the AI-Assisted Workflow

This project uses an AI agent (Claude Code) to assist with the drizzling pipeline.
This document explains how to work with it — what happens at the start of every
session, what each command does, and what to do when things go wrong.

---

## What This Project Does

Downloads WFC3/IR imaging for 6 quasars from HST proposal 14706 (PI: Glikman) via MAST,
aligns the exposures with TweakReg, and combines them into drizzled mosaics with
AstroDrizzle. The pipeline will later be extended to ACS optical data.

**Run the notebooks in this order:**
1. `notebooks/01_download_data.ipynb` — download FLT files from MAST
2. `notebooks/02_inspect_raw.ipynb` — check data quality
3. `notebooks/03_align_images.ipynb` — geometric alignment (TweakReg)
4. `notebooks/04_drizzle.ipynb` — combine exposures (AstroDrizzle)
5. `notebooks/05_inspect_outputs.ipynb` — review final mosaics

---

## Starting a Session

When you open Claude Code, the agent will automatically ask:

> "Would you like me to read HANDOFF.md to catch up on the project state?"

**Say yes** if you are continuing pipeline work. The agent reads `HANDOFF.md`,
summarizes where things stand, checks for unsaved work, and is ready to help —
no re-briefing needed.

**Say no** if you only want quick answers without any file changes.
This puts the agent in **advisory mode** (see below).

---

## Ending a Session

**Always run `/end-session` before closing the chat window.**

> `/end-session`

The agent summarizes what was accomplished, updates the pipeline status table,
writes next steps, and shows you the proposed `HANDOFF.md` update for approval.

For long sessions, also run `/checkpoint` after completing a major step — this
writes a mid-session snapshot without ending the session:

> `/checkpoint`

---

## Full Command Reference

### Session Management

| Command | What it does |
|---|---|
| `/end-session` | **Always run before closing the chat window.** Writes a full session summary to HANDOFF.md with pipeline state and next steps. |
| `/checkpoint` | Mid-session save state after completing a major step. Writes a progress snapshot to HANDOFF.md without ending the session. Use generously during long sessions. |
| `/reconstruct-session` | Run at the start of a session after a crash or accidental close. Uses git history and file state to rebuild a HANDOFF.md entry when /end-session was not run. |

### AI Interaction Modes

| Command | What it does |
|---|---|
| `/advisory-mode` | Switch to read-only mode. The agent answers questions and explains code but makes no changes to any files. Use when you want guidance without edits. |
| `/resume-editing` | Switch from advisory to editing mode. Reads HANDOFF.md, checks for unsaved work or concurrent sessions, and confirms it is safe to make changes before switching. |
| `/log-issue` | Permanently log a mistake the agent keeps making. Adds a dated entry to the Known Issues section of CLAUDE.md so every future session sees it automatically. |

### GitHub — Push, Pull, and Recovery

| Command | What it does |
|---|---|
| `/push-to-github` | Stage relevant files, draft a commit message, and push to the current branch. Always shows the diff summary and asks for approval before committing. |
| `/sync-from-github` | Safely sync from the remote when returning from another machine. Fetches without pulling, diagnoses whether branches have diverged, checks for conflict risk per file, and never merges without your approval. |

### Pipeline — This Project

| Command | What it does |
|---|---|
| `/pipeline-status` | Check which quasars have been downloaded, aligned, and drizzled. Produces a status table across all 6 targets. |
| `/validate-downloads` | After notebook 01 — verify all 6 quasars have the expected FLT files and none are missing or corrupted before processing begins. |
| `/rename-quasars` | After notebook 01 — replace quasar_01–06 placeholder folder names with real target names from MAST. Updates all directories and quasars.md. |
| `/check-alignment` | After notebook 03 — parse TweakReg logs for failed alignments or large residuals. Flags any quasars that may need re-alignment. |
| `/sync-params` | Check that config/wfc3_ir_drizzle_params.yaml matches what is actually used in 04_drizzle.ipynb. Flags drift between config and notebook. |
| `/review-notebooks` | Periodic cleanup — scan all notebooks for redundant code, hardcoded parameters, and inconsistencies. Reports only, no changes. |
| `/update-docs` | Refresh docs/quasars.md and docs/pipeline_overview.md to reflect the current state of the project. Shows proposed changes for approval. |
| `/summarize-results` | After drizzling is complete — generate a science results table across all 6 quasars and add it to docs/quasars.md. |
| `/add-instrument` | When ready to extend to ACS — scaffolds new notebooks, config, and data directories for the new instrument. Asks about download requirements from scratch. |

---

## Two Modes of Operation

### Editing Mode
The agent can read files, edit notebooks, update docs, and make changes to the
project. This is the normal working mode when you say yes at session start.

### Advisory Mode
The agent answers questions, explains code, and provides guidance — but makes
**no changes** to any files. Say no at session start, or run `/advisory-mode`
at any time.

To switch from advisory to editing mid-session:
> `/resume-editing`

The agent re-reads `HANDOFF.md` and checks for unsaved work before switching.

---

## Crash Recovery

If you forget `/end-session` or your machine crashes:

- A background process writes a timestamp to `.claude/session_heartbeat.txt`
  after every agent response
- At the start of the next session, the agent compares this timestamp to the
  last `HANDOFF.md` entry
- If the heartbeat is newer, unsaved work exists — the agent flags it and
  prompts you to run `/reconstruct-session`

You don't need to do anything special. The agent checks automatically.

---

## Concurrent Sessions (Two Windows Open)

The same heartbeat check catches when another window may be actively working.
If you run `/resume-editing` and unsaved work is detected, the agent will ask you to:

1. Check your other open Claude Code windows
2. Ask any active window to run `/checkpoint` to save its progress
3. Switch that window to `/advisory-mode`
4. Then run `/resume-editing` again here

**One editing window at a time.** Use advisory mode in any second window.

---

## Switching Machines

Before starting work after using another machine:
> `/sync-from-github`

| Scenario | What happens |
|---|---|
| Already up to date | Nothing to do |
| Remote is ahead, local is clean | Shows what changed, asks approval to pull |
| Local is ahead | Prompts you to push |
| **Both have new commits** | Full diagnostics — which files conflict, risk levels, recommended action |

The agent never merges or pulls without your explicit approval.

---

## Logging a Known Issue

If the agent makes the same mistake twice, log it so every future session
sees the warning automatically:
> `/log-issue`

The agent asks what went wrong, then adds a dated entry to the `Known Issues`
section of `CLAUDE.md`. These are loaded at the start of every session.

---

## Key Files

| File | What it is |
|---|---|
| `CLAUDE.md` | Loaded automatically every session. Project overview, conventions, and known issues. |
| `HANDOFF.md` | Running log of session summaries and checkpoints. Read automatically at session start. |
| `config/wfc3_ir_drizzle_params.yaml` | All drizzle parameters. Edit this file, not the notebook. |
| `docs/pipeline_overview.md` | Narrative description of the full pipeline |
| `docs/data_types.md` | Reference for FLT, FLC, DRZ, DRC file types |
| `docs/quasars.md` | Catalog of the 6 targets; updated as the pipeline runs |
| `.claude/session_heartbeat.txt` | Auto-written timestamp for crash and concurrent-session detection. Do not edit. |
| `.claude/commands/` | All command files. Open any to read exactly what a command does. |

---

## Project Structure

```
glikman_quasars_wfc3ir/
├── README.md                   ← you are here
├── CLAUDE.md                   ← AI context (loaded automatically)
├── HANDOFF.md                  ← session log (read automatically at session start)
├── .claude/
│   ├── commands/               ← all 17 slash commands
│   └── session_heartbeat.txt   ← auto-written; do not edit
├── docs/                       ← pipeline docs and quasar catalog
├── config/                     ← drizzle parameters
├── notebooks/                  ← 01 through 05, run in order
└── data/
    ├── raw/<quasar>/           ← downloaded FLT files (never modified)
    └── processed/<quasar>/
        ├── aligned/            ← FLT files after TweakReg
        └── drizzled/           ← final DRZ mosaics
```

---

*For pipeline details, see `docs/pipeline_overview.md`.*
*For questions about Claude Code itself, type `/help` in the chat panel.*
