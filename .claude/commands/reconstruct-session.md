# /reconstruct-session
# Rebuilds the session record from git history and file state when the previous
# session ended without /end-session (crash, accidental window close, etc.).
# Run this at the start of a session if the heartbeat is newer than HANDOFF.md.

## When to run this
Run this if, at session start, you notice:
- .claude/session_heartbeat.txt shows a timestamp significantly newer than the
  "Last updated" date in HANDOFF.md (more than a few hours means work happened
  that was never recorded)

## Step 1 — Triage: was this just an advisory session?

Before doing a full reconstruction, quickly check whether any real work happened:

1. Run `git log --since="<HANDOFF.md Last updated date>" --oneline`
2. Check which files were modified since that date:
   `find . -newer HANDOFF.md -not -path "./.git/*" -not -path "./data/*"`

**If the only changes are TODO.md (and/or no git commits exist):**
This was an advisory session — no pipeline work was lost.
Tell the user:
> "The heartbeat is newer than HANDOFF.md, but the only change since the last
> update is TODO.md. This looks like an advisory session — no pipeline work lost.
> State is clean."
Then proceed normally. Do NOT write a reconstruction entry.

**If git commits exist or other files were modified:**
Proceed to full reconstruction below.

## Step 2 — Full reconstruction

1. **Check the heartbeat:** Read `.claude/session_heartbeat.txt` for the last
   active timestamp

2. **Check git history:** Review commits made since the HANDOFF.md Last updated date

3. **Check file modification times:** Note what changed in notebooks/, config/, docs/

4. **Check data directory state:** Inspect data/raw/ and data/processed/ to determine
   current pipeline state for all 6 quasars

5. **Check sessions/ for the most recent file:** If a sessions/<DATE>_session.md exists
   for the missing period, use it as additional evidence

## Step 3 — Write the reconstructed record

Create sessions/<DATE>_session.md [RECONSTRUCTED]:

### Session — <DATE> [RECONSTRUCTED — session ended without /end-session]
**What likely happened (reconstructed from git + file state):**
- ...

**Evidence used:**
- git commits: ...
- modified files: ...
- pipeline state change: ...

**Confidence:** low / medium / high

Then overwrite HANDOFF.md with the current state snapshot (same format as /end-session).

Show the proposed sessions/ entry and the proposed HANDOFF.md for user confirmation
before writing — the user should verify the reconstruction looks right since it is
inferred rather than directly observed.
