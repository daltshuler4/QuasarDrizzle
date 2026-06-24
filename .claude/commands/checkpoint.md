# /checkpoint
# Updates HANDOFF.md in-place with current state. No session archive is created —
# that only happens at /end-session. Use this during long sessions to keep HANDOFF.md
# current in case of a crash or concurrent window.

Write a mid-session checkpoint by updating HANDOFF.md with the current state:

## When to run this
- After completing a significant step during a long session
- Before switching tasks or handing off to another window
- Any time you want a save state in case of a crash
- When another window needs to know your current progress

## What to do
1. Inspect data/raw/ and data/processed/ to get the current pipeline state
2. Note what has been accomplished since the last HANDOFF.md update (3–5 bullets)
3. Note any work currently in progress (e.g. "alignment running for quasar_03")
4. Overwrite HANDOFF.md with the updated current state:

# Project Handoff — Current State

> This file is always the current snapshot. Full session history is in `sessions/`.
> Read the most recent file there for narrative context, or look back further if needed.

## Last updated — <DATE> <TIME> (checkpoint)

**Progress since last update:**
- ...

**Pipeline state:**
| Quasar | Downloaded | Aligned | Drizzled |
|--------|-----------|---------|----------|
...

**In progress:**
- ...

**Open issues:**
- ...

**Next steps:**
1. ...

Show the proposed HANDOFF.md for approval before writing.
The session continues after this — do not stop working unless the user says to.
