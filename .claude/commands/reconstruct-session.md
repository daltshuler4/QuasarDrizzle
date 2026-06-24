# /reconstruct-session
# Rebuilds a HANDOFF.md session entry from git history and file state when the
# previous session ended without /end-session (crash, accidental window close, etc.).
# Run this at the start of a session if HANDOFF.md looks stale.

Reconstruct what happened in the previous session and write a recovery entry to HANDOFF.md.

## When to run this
Run this command if, at the start of a session, you notice:
- HANDOFF.md has no entry for the most recent date
- .claude/session_heartbeat.txt shows a timestamp significantly newer than the
  last HANDOFF.md session entry (more than a few hours newer means work happened
  that was never summarized)

## Reconstruction steps

1. **Check the heartbeat:** Read `.claude/session_heartbeat.txt` to find when the
   last session was active

2. **Check git history:** Run `git log --since="<last HANDOFF date>" --oneline` to
   see if any commits were made in the missing period

3. **Check file modification times:** Look at what files in notebooks/, config/,
   and docs/ were modified since the last HANDOFF.md entry date

4. **Check data directory state:** Inspect data/raw/ and data/processed/ to see
   the current pipeline state for all 6 quasars (compare to last known state in
   HANDOFF.md to infer what processing may have run)

5. **Reconstruct the summary:** Based on the above evidence, write a recovery
   HANDOFF.md entry clearly marked as reconstructed:

## Session — <DATE> [RECONSTRUCTED — session ended without /end-session]
**What likely happened (reconstructed from git + file state):**
- ...

**Pipeline state:**
| Quasar | Downloaded | Aligned | Drizzled |
...

**Next steps:**
1. Verify the reconstructed state is accurate before proceeding
2. ...

Prepend this entry at the TOP of HANDOFF.md. Show the proposed reconstruction
for user confirmation before writing — the user should verify it looks right
since it is inferred rather than directly observed.
