# AI-Friendly Project Setup Template

Share this file with an AI agent at the start of any new project and say:
> "Use `AI_PROJECT_SETUP_TEMPLATE.md` to set up this project. Follow the discovery
> conversation before proposing any structure."

The agent will ask you questions first, propose a structure tailored to your project,
and wait for your approval before creating any files.

---

## How This Works

Setting up a project well takes a short conversation. The agent should NOT immediately
propose a generic structure — it should first understand your project, then design
something specific to it. The structure, commands, and docs in this template are
examples of what a completed setup looks like, not a blueprint to copy.

---

## Step 1 — Discovery Conversation

Before proposing anything, the agent must ask the user these questions:

**About the project:**
1. What is the goal of this project? What does a finished result look like?
2. Do you have any documentation, papers, or guides describing the process or
   pipeline you will be following? If so, please share them — the agent should
   read them before proposing a structure.
3. What is the data source? (e.g. a web archive, local files, a database, an API)
4. How many targets, objects, samples, or subjects are there?
5. What are the main processing steps, in order?
6. What file types will you be working with as inputs and outputs?

**About workflow:**
7. Will you be working on this across multiple machines?
8. Are there any known pitfalls, constraints, or mistakes to be aware of from
   the start? (These become the first Known Issues entries in CLAUDE.md.)
9. Are there any existing notebooks, scripts, or repos to reference or build from?
   If so: is it a public repo the user has forked, or a private repo they own?
   (This determines whether it should be added as a git submodule — see
   External Reference Repos below.)

**About commands:**
10. Walk through the project's workflow together and identify tasks that are:
    - Repetitive (you'll do them many times)
    - Easy to get wrong without a checklist
    - Time-consuming but mechanical
    - Good checkpoints for verifying the AI's work before moving on
    These become the project-specific commands. See the command categories below.

Only after this conversation should the agent propose a structure.

---

## Step 2 — Propose Structure for Approval

After the discovery conversation, the agent proposes:
1. The full directory structure (show as a tree, with every file explained)
2. The complete command list (universal + project-specific)
3. Any docs files to create

**Show everything before building anything.** The user must explicitly approve
the structure before the agent creates a single file.

---

## Step 3 — Build

After approval:
1. Create all directories with `.gitkeep` files so git tracks empty folders
2. Create `CLAUDE.md` filled in with project-specific content
3. Create `HANDOFF.md` with the initial session entry
4. Create all command files — universal and project-specific
5. Create doc stubs and config file
6. Create numbered notebook stubs

---

## Philosophy

An AI-friendly project is organized so that any agent — in any session — can
understand the full context without being re-briefed:

1. **CLAUDE.md** — loaded automatically every session; contains the project overview,
   conventions, and a "Known Issues" section to prevent repeated mistakes
2. **HANDOFF.md** — always the current state snapshot (~40 lines max); overwritten at
   each `/end-session` and `/checkpoint`, never appended to. Full session narratives
   live in `sessions/` — the agent reads HANDOFF.md every time, and looks into
   `sessions/` only when deeper context is needed
3. **Numbered notebooks** — enforce the correct order of operations at a glance
4. **Config files** — all tunable parameters in one versioned file, not scattered
   across notebook cells
5. **Commands** — slash commands for repetitive, time-consuming, or verification tasks
6. **TODO.md** — a single file for ideas and future work that aren't being implemented
   yet; the only file the agent may edit in advisory mode, so no idea is ever lost
   regardless of what mode the agent is in

---

## Universal Commands

These go in every project regardless of domain. Create each as a `.md` file in
`.claude/commands/` — the filename becomes the slash command.

---

### Session Management

**`end-session.md`** — `/end-session`
```
Write a structured session summary and update the handoff state.

Step 1 — Write full session detail to sessions/<DATE>_session.md:
  - What was accomplished (3-5 specific bullet points)
  - Decisions made and why
  - Open issues at end of session
  This file is the permanent record — write with enough detail that a future
  agent reading it months from now would understand what happened and why.

Step 2 — Overwrite HANDOFF.md with current state snapshot only:
  - "Last updated" date
  - Pipeline/progress state table for each target
  - Open issues
  - Numbered, specific, actionable next steps
  HANDOFF.md is NOT a log — it is always the current state only.

Show both proposed files for approval before writing either.

After writing both files, ask:
  "Would you like me to push these changes to GitHub?"
  If yes: run the /push-to-github flow — show staged files and commit message
  for approval before committing or pushing anything.
```

**`checkpoint.md`** — `/checkpoint`
```
Update HANDOFF.md in-place with current state. No session archive is created —
that only happens at /end-session.

Use this after completing a significant step, before switching tasks, or any
time you want a save state during a long session.

Overwrite HANDOFF.md with updated current state: "Last updated" date + time
(checkpoint), progress since last update, pipeline state table, anything
currently in progress, open issues, and next steps.

Show the proposed HANDOFF.md for approval before writing.
The session continues after this runs.
```

**`reconstruct-session.md`** — `/reconstruct-session`
```
Rebuild the session record when the previous session ended without /end-session.

Run this if session_heartbeat.txt is newer than the "Last updated" date in
HANDOFF.md, indicating unsaved work from a crash or accidental close.

Step 1 — Triage first (advisory session check):
  Run git log --since="<HANDOFF.md Last updated date>" --oneline and check
  which files were modified since that date.
  - If only TODO.md changed and no git commits exist: this was an advisory
    session — no pipeline work was lost. Tell the user and proceed normally.
    Do NOT write a reconstruction entry.
  - If git commits exist or other files were modified: proceed to full
    reconstruction below.

Step 2 — Full reconstruction (only if Step 1 finds real changes):
  1. Read session_heartbeat.txt for the last active timestamp
  2. Review git commits made since the HANDOFF.md Last updated date
  3. Check file modification times in notebooks/, config/, and docs/
  4. Inspect data/processed/ to determine current pipeline state
  5. Check sessions/ for any existing file from the missing period
  Write sessions/<DATE>_session.md [RECONSTRUCTED] with the inferred summary,
  evidence used, and confidence level. Then overwrite HANDOFF.md with the
  current state snapshot.
  Show both for user confirmation before writing.
```

---

### AI Interaction Modes

**`advisory-mode.md`** — `/advisory-mode`
```
Switch to read-only advisory mode for this session.

In advisory mode:
- Answer questions, explain code, read files, and provide guidance
- Do NOT edit any files, notebooks, configs, or run commands that write to disk
- If asked to make a change, say: "I'm in advisory mode — run /resume-editing
  if you'd like me to make changes"

Advisory mode is useful when you want to understand the project without
changing it, or get a second opinion before committing to an approach.
To return to editing mode, run /resume-editing.
```

**`resume-editing.md`** — `/resume-editing`
```
Switch from advisory mode to full editing mode with safety checks.

Steps:
1. Read HANDOFF.md and summarize the current state in 2-3 sentences
2. Compare session_heartbeat.txt to the last HANDOFF.md entry date:
   - If the heartbeat is NEWER than the last entry, unsaved work exists.
     This could mean a crash or an active session in another window.
     Tell the user and ask them to either run /reconstruct-session (crash)
     or ask the other window to /checkpoint before proceeding (concurrent session).
   - If the heartbeat matches or is older, state is clean — proceed.
3. Confirm: "HANDOFF.md is current, no unsaved work detected. Switching to editing mode."

Never have two windows in editing mode at the same time.
```

**`log-issue.md`** — `/log-issue`
```
Add a known issue to the "Known Issues — Always Check Before Acting" section
of CLAUDE.md. This section is loaded at the start of every session.

Ask the user:
1. What mistake or incorrect behavior occurred?
2. What is the correct behavior, or what should be checked instead?

Append a dated entry:
  - [ ] Issue logged <DATE>: <what to watch out for and why>

Show the entry for approval before writing. Do not modify any other part of CLAUDE.md.
```

---

### GitHub

**`push-to-github.md`** — `/push-to-github`
```
Stage relevant files, write a commit message, and push to the current branch.

1. Run git status to show what has changed
2. Exclude data/ and any large binary files (gitignored)
3. Use HANDOFF.md context to draft a clear, specific commit message
4. Show the staged files and commit message for approval
5. After approval: stage, commit, and push

Never push data files. Never force push. Always show the diff summary first.
```

**`sync-from-github.md`** — `/sync-from-github`
```
Safely sync with the remote GitHub repository when returning from another machine.

1. Check for uncommitted local changes first — warn and ask how to handle before continuing
2. Run git fetch (does not change any local files)
3. Diagnose the situation:
   - Already up to date: report and stop
   - Remote ahead, local clean (fast-forward): show what changed, ask approval to pull
   - Local ahead: prompt to push via /push-to-github
   - DIVERGED (both have new commits): run full diagnostics —
       * Show commits unique to each side
       * List files changed on both sides with conflict risk:
           HIGH: notebooks, config files
           MEDIUM: HANDOFF.md, CLAUDE.md, docs
       * Recommend: safe to merge / needs manual review / do not merge
4. Never run git merge or git pull without explicit user approval.
5. After any pull, note the sync in HANDOFF.md.
```

---

## Project-Specific Commands

After the discovery conversation, design commands tailored to this project.
The goal is to give the user easy ways to verify the AI's work and check
the state of the data at key milestones. Every data-intensive project benefits
from commands in some or all of these categories:

---

### Category 1 — Data Validation
*Verify that inputs are complete and correct before running expensive steps.*

Ask the user: what would tell you that a download or import went wrong?
What does a complete, healthy set of inputs look like for one target?

Design a command that checks all targets against that definition and flags
anything missing, corrupted, or inconsistent. This command should run between
the download step and the first processing step.

> **Example from a completed project:** `/validate-downloads` checked that all
> 6 quasars had the expected number of WFC3/IR FLT files, verified file extensions,
> and spot-checked FITS headers for the correct instrument and proposal ID.

---

### Category 2 — Pipeline Status
*Check where each target stands across all processing stages at a glance.*

Ask the user: what are the discrete stages a target passes through? What file
or folder would prove a stage is complete?

Design a command that inspects the project's directory structure and produces
a status table — one row per target, one column per stage, checkmarks for done.

> **Example from a completed project:** `/pipeline-status` scanned data/raw/ and
> data/processed/ for all 6 quasars and reported which had been downloaded, aligned,
> and drizzled in a single table.

---

### Category 3 — AI Work Review
*Let the user verify what the AI produced before accepting it and moving on.*

Ask the user: at which steps is it most important to check the AI's output?
What does a bad result look like at each of those steps?

Design commands that read logs, compare outputs across targets, or check
quality metrics and flag anything that looks wrong. These commands report only
— they never make changes. They exist so the user stays in control at critical
checkpoints.

> **Example from a completed project:** `/check-alignment` parsed TweakReg logs
> for all quasars and flagged any with large RMS residuals or failed fits before
> the user proceeded to drizzling. `/notebook-check` scanned all notebooks for
> redundant code and hardcoded parameters.

---

### Category 4 — Config Consistency
*Prevent drift between parameter files and notebooks.*

If the project has a config file with tunable parameters, design a command
that checks the config against the notebooks and flags any parameters that
are hardcoded in notebooks instead of being read from config, or any config
parameters that are never referenced anywhere.

> **Example from a completed project:** `/sync-params` compared
> `config/wfc3_ir_drizzle_params.yaml` against `04_drizzle.ipynb` and flagged
> drift between the two after iterative parameter tuning.

---

### Category 5 — Output Summary
*Generate a science or results summary across all targets at the end.*

Ask the user: what metrics matter at the end of the pipeline? What would you
want in a summary table to compare results across all targets?

Design a command that collects those metrics from the final outputs and writes
a results table to the project's docs.

> **Example from a completed project:** `/summarize-results` generated a table
> of exposure time, pixel scale, and image dimensions for all 6 drizzled quasar
> mosaics and wrote it to docs/quasars.md.

---

### Category 6 — Project Expansion
*Scaffold new instruments, datasets, or processing variants without disrupting existing work.*

If the project scope is likely to grow (new data type, new instrument, new
targets), design a command that scaffolds the extension cleanly — new directories,
new config, new notebooks — while asking the user about any differences in the
new workflow rather than assuming anything carries over.

> **Example from a completed project:** `/add-instrument` scaffolded ACS optical
> processing alongside the existing WFC3/IR pipeline. It was explicitly instructed
> to build the download notebook from scratch after asking the user about the new
> query parameters, since file types and archive criteria differ between instruments.

---

## External Reference Repos

If the project depends on an external repository (e.g. a library of utility notebooks,
a reference implementation, or a tool that must be cloned whole), ask the user how
they want to handle it before doing anything. Present these options:

| Option | How it works | Portability |
|---|---|---|
| **Git submodule** | Embeds the repo inside this project at a pinned commit | Full — `git clone --recurse-submodules` gets everything |
| **Relative path** | Both repos cloned side-by-side; notebooks add the path via `sys.path` | Fragile — depends on folder layout being identical on every machine |
| **Install as package** | `pip install -e ../other-repo` — importable anywhere | Requires the other repo to be present and installed separately |
| **Copy needed code** | Paste the relevant functions directly into this project | Fully self-contained, but you now maintain two copies |

**Recommended default: git submodule** — it's the only option that makes the
dependency explicit in git so any machine that clones the project gets it automatically.

If the user chooses submodule, ask whether to point at:
- Their **own fork** of the repo (recommended — they control the version)
- The **upstream repo** directly (simpler, but they don't control update timing)

**How to add a submodule:**
```bash
git submodule add <remote-url> <local-folder-name>
git commit -m "Add <repo-name> as git submodule"
```

**On a new machine**, the submodule is included automatically:
```bash
git clone --recurse-submodules <project-url>
```

**Updating the pin** (when the external repo releases something you want):
```bash
cd <submodule-folder> && git pull && cd ..
git add <submodule-folder>
git commit -m "Update <repo-name> submodule to latest"
```

The submodule never updates itself automatically — you always choose when to
advance the pin. This is intentional for reproducibility.

---

## Persistent Memory

Claude Code supports a file-based persistent memory system at
`.claude/projects/<project-path>/memory/`. Memories written here are loaded into
every future session automatically.

**Use memory to save:**
- Documentation URLs (handbook PDFs, API references, notebook guides)
- User preferences and workflow feedback
- Project decisions and their rationale that aren't obvious from the code

**Memory file format:**
```markdown
---
name: <short-kebab-case-slug>
description: <one-line summary>
metadata:
  type: reference  # or: user, feedback, project
---

<memory content>
```

**Index file:** Also maintain a `MEMORY.md` index at the memory root — one line per
entry — so the agent can find relevant memories at a glance.

**What NOT to save in memory:** code patterns, file paths, git history, or anything
already in CLAUDE.md. Memory is for things that won't be rediscoverable from the
repo itself — external URLs, preferences, and context that lives outside the codebase.

---

## Crash / Missed End-Session Failsafe

Add this to every project. The Stop hook fires after every agent response and
writes a timestamp — if the session ends without `/end-session`, the timestamp
outlives the session and can be compared to HANDOFF.md at the next startup.

**Add to `.claude/settings.json` at the workspace root:**
```json
{
  "hooks": {
    "Stop": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "date '+%Y-%m-%d %H:%M:%S' > /absolute/path/to/project/.claude/session_heartbeat.txt"
          }
        ]
      }
    ]
  }
}
```

**How it works:**
- After every agent response, the heartbeat file is overwritten with the current time
- At session start, the agent compares heartbeat to the last HANDOFF.md entry
- If heartbeat is newer → unsaved work exists → agent flags it and prompts `/reconstruct-session`
- The same check catches concurrent sessions: if the heartbeat is newer than HANDOFF.md
  and you just opened a fresh window, another window may still be active

**Known limitation — path fragility:** The hook command uses an absolute path, which
means it will break on any machine where the project lives at a different location.
This is a known portability problem. Before using this project on a new machine,
update the path in `.claude/settings.json` to match the local project location.
Document this in your README.md Known Issues section so it isn't forgotten.

---

## Example Directory Structure

The structure below is an example from a data pipeline project with multiple targets.
Adapt it to your project — the key principles are what matter, not the exact layout.

**Key principles:**
- Raw inputs live in `data/raw/` and are never modified after download
- One folder per target throughout — raw and processed mirror each other
- Each processing stage has its own subdirectory so outputs are never overwritten
- All large binary files are gitignored; only code, config, and docs are tracked
- Numbered notebooks enforce the correct order of operations

```
<PROJECT_ROOT>/
│
├── README.md                          ← Human guide to the AI workflow
├── CLAUDE.md                          ← AI context, loaded every session
├── HANDOFF.md                         ← Current state snapshot; always ~40 lines
├── TODO.md                            ← Ideas and future work; never loses an idea
│
├── sessions/                          ← Full session narratives, one file per session
│   ├── YYYY-MM-DD_session.md          ← Written at /end-session; never modified after
│   └── ...
│
├── .claude/
│   ├── session_heartbeat.txt          ← Auto-written by Stop hook; do not edit
│   └── commands/
│       ├── end-session.md
│       ├── checkpoint.md
│       ├── reconstruct-session.md
│       ├── advisory-mode.md
│       ├── resume-editing.md
│       ├── log-issue.md
│       ├── push-to-github.md
│       ├── sync-from-github.md
│       └── <project-specific commands>
│
├── docs/
│   ├── pipeline_overview.md           ← Full workflow narrative
│   ├── data_types.md                  ← Input/output file type reference
│   └── targets.md                     ← Catalog of targets/objects/samples
│
├── config/
│   └── <project>_params.yaml          ← All tunable parameters; read by notebooks
│
├── notebooks/
│   ├── 01_<first_step>.ipynb
│   ├── 02_<second_step>.ipynb
│   ├── 03_<third_step>.ipynb
│   └── ...
│
└── data/
    ├── raw/
    │   ├── <target_01>/
    │   ├── <target_02>/
    │   └── ...
    └── processed/
        ├── <target_01>/
        │   ├── <stage_1>/
        │   └── <stage_2>/
        └── ...
```

---

## CLAUDE.md Template

```markdown
# <PROJECT_NAME>

## Session Start — Do This Before Anything Else

At the very start of every session, ask the user:

> "Would you like me to read HANDOFF.md to catch up on the project state?
> I strongly recommend this if you're continuing work — it ensures you won't
> need to re-explain where things stand.
>
> If you'd prefer I work in **advisory mode** (answering questions and providing
> context without making any changes), just say no."

**If the user says yes — Editing Mode:**
1. Read HANDOFF.md and summarize current state in 2-3 sentences
2. Compare `.claude/session_heartbeat.txt` to the "Last updated" date in HANDOFF.md:
   - If the heartbeat is NEWER than the Last updated date, unsaved work exists —
     tell the user and ask whether this was a crash (run /reconstruct-session) or
     whether another window may be active (ask them to /checkpoint it first)
   - If the heartbeat matches or is older, state is clean — proceed normally
3. If more context is needed beyond HANDOFF.md, read the most recent file in sessions/
4. Ask how you can help

**If the user says no — Advisory Mode:**
- Answer questions, explain code, read files, and provide guidance only
- Do NOT edit any files, notebooks, configs, or run any commands that write to disk
- If asked to make a change, say: "I'm in advisory mode — run /resume-editing if
  you'd like me to make changes"
- /resume-editing will re-run the safety checks above before switching modes

## Project Overview
<2-3 sentences: goal, data source, end product>

## Data Source
- Source: <archive, API, local, etc.>
- Query / access parameters: <identifiers, filters, criteria>
- Number of targets: <N>
- File types: <input and output formats>

## Pipeline Steps
1. `01_....ipynb` — <what it does>
2. `02_....ipynb` — <what it does>
...

## Key Parameters
All parameters live in `config/<project>_params.yaml`.
Never hardcode parameters in notebooks — always read from config.

## Conventions
- Notebooks are numbered and must be run in order
- One folder per target in data/raw/ and data/processed/
- Always read HANDOFF.md before starting any work

## Available Commands
| Command | Purpose |
|---|---|
| `/end-session` | Write session summary to HANDOFF.md |
| `/checkpoint` | Mid-session save state |
| `/reconstruct-session` | Rebuild HANDOFF.md after a crash |
| `/advisory-mode` | Switch to read-only mode |
| `/resume-editing` | Switch to editing mode with safety checks |
| `/log-issue` | Log a recurring mistake permanently |
| `/push-to-github` | Stage, commit, and push |
| `/sync-from-github` | Safely pull from remote with conflict diagnostics |
| <project-specific commands> | ... |

## Known Issues — Always Check Before Acting
<!-- Entries added via /log-issue -->
```

---

## HANDOFF.md Template

HANDOFF.md is always the current state snapshot — not a log. It gets overwritten at
each `/end-session` and `/checkpoint`. Full session narratives live in `sessions/`.

```markdown
# Project Handoff — Current State

> This file is always the current snapshot. Full session history is in `sessions/`.
> Read the most recent file there for narrative context, or look back further if needed.

## Last updated — <DATE>

**Progress state:**
| Target | <Stage 1> | <Stage 2> | <Stage 3> | Final Output |
|--------|-----------|-----------|-----------|--------------|
| target_01 | ✗ | ✗ | ✗ | ✗ |

**Open issues:**
-

**Next steps:**
1.
```

## sessions/ Template

Each session file is written once at `/end-session` and never modified after.

```markdown
# Session — <DATE>

**What we did:**
-

**Decisions made:**
-

**Open issues at end of session:**
-
```

---

*Template version: 2.0 — 2026-06-24*
