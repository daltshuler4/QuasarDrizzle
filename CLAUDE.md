# Glikman Quasars — WFC3/IR Drizzling Pipeline

## Project Overview
This project downloads, aligns, and drizzle-combines HST WFC3/IR imaging for 6 quasars
from proposal 14706 (PI: Glikman). The goal is to produce science-ready drizzled mosaics
for each target using AstroDrizzle. The pipeline will later be extended to ACS optical data.

## Data Source
- Archive: MAST (Mikulski Archive for Space Telescopes)
- Proposal ID: 14706
- PI: Glikman
- Instrument: WFC3/IR
- Project: HST (not HAP — HAP returns processed mosaics, not individual exposures)
- File type: FLT (calibrated individual exposures from CALWF3)
- Number of targets: 6 quasars

## Pipeline Steps
1. `01_download_data.ipynb` — query MAST by proposal ID 14706, filter to WFC3/IR FLT files,
   download to data/raw/<quasar>/, verify 6 distinct targets are returned
2. `02_inspect_raw.ipynb` — examine FITS headers, plot raw images, check exposure times
   and image quality before committing to processing
3. `03_align_images.ipynb` — run TweakReg to correct geometric alignment across exposures;
   outputs updated FLT files saved to data/processed/<quasar>/aligned/
4. `04_drizzle.ipynb` — run AstroDrizzle per quasar using parameters from config/;
   outputs DRZ files saved to data/processed/<quasar>/drizzled/
5. `05_inspect_outputs.ipynb` — review drizzled products, check image quality,
   confirm astrometry, generate summary figures

## Key Parameters
All drizzle parameters live in `config/wfc3_ir_drizzle_params.yaml`.
Never hardcode parameters in notebooks — always read from config.

## Directory Layout
```
data/raw/<quasar>/          ← downloaded FLT files, never modified after download
data/processed/<quasar>/
    aligned/                ← FLT files with updated WCS after TweakReg
    drizzled/               ← final DRZ mosaics from AstroDrizzle
```

## Session Start — Do This Before Anything Else

At the very start of every session, ask the user:

> "Would you like me to read HANDOFF.md to catch up on the project state?
> I strongly recommend this if you're continuing pipeline work — it ensures
> you won't need to re-explain where things stand.
>
> If you'd prefer I work in **advisory mode** (answering questions and providing
> context without making any changes), just say no."

**If the user says yes — Editing Mode:**
1. Read HANDOFF.md and summarize current pipeline state in 2-3 sentences
2. Compare `.claude/session_heartbeat.txt` to the "Last updated" date in HANDOFF.md:
   - If the heartbeat is NEWER than the Last updated date, unsaved work exists —
     tell the user and ask whether this was a crash (run /reconstruct-session) or
     whether another window may be active (ask them to /checkpoint it first)
   - If the heartbeat matches or is older, state is clean — proceed normally
3. Ask how you can help

**If the user says no — Advisory Mode:**
- Answer questions, explain code, read files, and provide guidance only
- Do NOT edit any files, notebooks, configs, or run any commands that write to disk
- Exception: you MAY append to TODO.md in advisory mode — it exists to capture ideas
  regardless of mode
- If asked to make any other change, say: "I'm in advisory mode — run /resume-editing
  if you'd like me to make changes"
- /resume-editing will re-run the safety checks above before switching modes

## Conventions
- Notebooks are numbered and must be run in order
- One folder per quasar in both data/raw/ and data/processed/
- Placeholder names (quasar_01 through quasar_06) are replaced by /rename-quasars
  once actual target names are confirmed from the MAST query
- Always read HANDOFF.md before starting any work session
- All large files (.fits, .log) are gitignored — only code and config are tracked

## Handoff Structure
- **HANDOFF.md** — always the current state snapshot only (~40 lines). Overwritten at
  each /end-session and /checkpoint. Never a log — read this first every session.
- **sessions/** — one file per session (`sessions/YYYY-MM-DD_session.md`), written at
  /end-session. Full narrative detail lives here. Read the most recent file for context;
  look back further if needed.

## Crash / Missed End-Session Recovery
A Stop hook automatically writes a timestamp to `.claude/session_heartbeat.txt`
after every agent response. At the start of each session:

1. Check `.claude/session_heartbeat.txt` — this shows when the last session was active
2. Compare that timestamp to the "Last updated" date in HANDOFF.md
3. If the heartbeat is more than a few hours newer, run /reconstruct-session —
   it will first check whether the gap is just advisory activity (TODO.md only,
   no git commits). If so, it clears automatically. If real work is found, it
   walks through full reconstruction before proceeding.

## Available Commands
| Command | Purpose |
|---|---|
| `/notebook-check` | Scan notebooks for redundant code, hardcoded parameters, inconsistencies |
| `/push-to-github` | Stage, commit, and push current work |
| `/pipeline-status` | Check which quasars are at which pipeline stage |
| `/validate-downloads` | Verify FLT files before processing |
| `/check-alignment` | Parse TweakReg logs for alignment issues |
| `/rename-quasars` | Replace quasar_01–06 with real target names |
| `/sync-params` | Check config YAML matches notebook parameters |
| `/update-docs` | Refresh docs/ from current notebook/data state |
| `/summarize-results` | Generate results table across all 6 quasars |
| `/add-instrument` | Scaffold ACS or other instrument extension |
| `/end-session` | Write session detail to sessions/, overwrite HANDOFF.md with current state |
| `/log-issue` | Append a known issue to this file |
| `/reconstruct-session` | Rebuild session record from git history after a crash |
| `/sync-from-github` | Safely pull from remote, diagnose divergence, check for conflicts |
| `/advisory-mode` | Switch to read-only mode — answers questions, no edits |
| `/resume-editing` | Switch from advisory to editing mode with safety checks |
| `/checkpoint` | Update HANDOFF.md in-place with current state (no session archive) |

## Known Issues — Always Check Before Acting
<!-- Entries added via /log-issue -->
- [ ] Always query MAST with project='HST', not 'HAP'. HAP returns processed mosaics;
      we need individual FLT exposures from CALWF3.
