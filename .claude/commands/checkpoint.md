# /checkpoint
# Writes a mid-session progress snapshot to HANDOFF.md without ending the session.
# Use this during long sessions, before switching tasks, or when another window
# may need to know your current progress.

Write a mid-session checkpoint entry to HANDOFF.md.

A checkpoint is lighter than /end-session — it records the current state without
implying the session is over. Use it:
- During a long session after completing a significant step
- Before handing off to another window or machine
- Any time you want a save state in case of a crash
- When another window asks for your current progress before it switches to editing mode

## What to write

1. Inspect data/raw/ and data/processed/ to get the current pipeline state
2. Summarize what has been accomplished since the last HANDOFF.md entry (3-5 bullets)
3. Note any work currently in progress (e.g. "alignment running for quasar_03")
4. Write the entry marked clearly as a checkpoint, not a session end:

## Checkpoint — <DATE> <TIME>
**Progress since last entry:**
- ...

**Pipeline state:**
| Quasar | Downloaded | Aligned | Drizzled |
|--------|-----------|---------|----------|
...

**In progress:**
- ...

**Note:** Session still active — this is a mid-session checkpoint, not an end-of-session summary.

Prepend this at the TOP of HANDOFF.md so it is the first thing the next reader sees.
Show the proposed checkpoint for approval before writing.
The session continues after this — do not stop working unless the user says to.
