# /sync-from-github
# Safely syncs the project with the remote GitHub repository when returning from
# another machine. Diagnoses divergence, checks for conflicts, and never merges
# without explicit user approval.

Sync this project with the remote GitHub repository and report on any divergence.

## Step 1 — Check local state
Before touching anything:
1. Run `git status` — are there any uncommitted local changes?
2. If yes: warn the user and ask whether to stash, commit, or abort before continuing
   Never proceed past this step with uncommitted changes unless the user says to

## Step 2 — Fetch (do not pull yet)
Run `git fetch origin` to retrieve remote state without changing any local files.
This is safe — it only updates git's knowledge of the remote, not the working tree.

## Step 3 — Diagnose the relationship
Compare local branch to remote branch and categorize:

**Case A — Already up to date:**
  Report: "Local and remote are identical. Nothing to sync."
  Done.

**Case B — Remote is ahead, local has no new commits (fast-forward):**
  Report: what commits exist on remote that local doesn't have
  Show a summary of changed files using `git diff HEAD..origin/<branch> --name-only`
  Highlight if any of these were changed: notebooks/, config/, CLAUDE.md, HANDOFF.md
  Ask for approval, then run `git pull --ff-only`

**Case C — Local is ahead, remote has no new commits:**
  Report: what commits exist locally that remote doesn't have
  Ask the user if they want to push via /push-to-github
  Done.

**Case D — DIVERGED (both local and remote have new commits):**
  This is the complex case — run full diagnostics before recommending anything:

  1. Show commits that exist only on remote: `git log HEAD..origin/<branch> --oneline`
  2. Show commits that exist only on local: `git log origin/<branch>..HEAD --oneline`
  3. Identify files changed on BOTH sides: `git diff origin/<branch>...HEAD --name-only`
  4. For each file changed on both sides, assess conflict risk:
     - notebooks/*.ipynb — HIGH RISK (JSON format, cell outputs make diffs messy)
     - config/wfc3_ir_drizzle_params.yaml — HIGH RISK (parameter conflicts could corrupt a run)
     - HANDOFF.md — MEDIUM RISK (can be manually merged)
     - CLAUDE.md — MEDIUM RISK (Known Issues section may have new entries on both sides)
     - docs/*.md — LOW RISK (usually easy to merge)
     - data/ — NOTE: data files should be gitignored; flag if any appear here
  5. Check if the remote work was branched from an older commit than the current local HEAD
     using `git merge-base HEAD origin/<branch>` — if the common ancestor is old,
     describe how far back the divergence goes

  6. Present a full diagnostic report:
     ## Sync Diagnostic Report
     - Diverged since: <common ancestor commit + date>
     - Remote-only commits: <list>
     - Local-only commits: <list>
     - Files changed on BOTH sides: <list with risk level>
     - Recommended action: <see below>

  7. Recommend one of:
     - SAFE TO MERGE: no shared files changed — recommend `git merge origin/<branch>`
     - MANUAL REVIEW NEEDED: shared files changed — list exactly what to check before merging
     - DO NOT MERGE: config or notebooks changed on both sides — recommend resolving on one
       machine first, then syncing the other

## Step 4 — After any merge or pull
1. Run `git status` to confirm clean state
2. Note the sync event at the top of HANDOFF.md:
   `Synced from GitHub at <timestamp> — see git log for changes pulled`
3. If HANDOFF.md was updated on the remote, flag that the user should read the new entries

## Golden rule
Never run `git merge`, `git pull`, or `git rebase` without explicit user approval.
Show the full diagnostic report first, state your recommendation, then wait.
