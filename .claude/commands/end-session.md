# /end-session
# Writes a full session summary to sessions/<DATE>_session.md, then updates
# HANDOFF.md with the current state snapshot. Always shows both for approval first.

Write a structured end-of-session summary and update the handoff state:

## Step 0 — Get the user's account
Before doing anything else, ask the user two questions:
1. "What happened this session?" — ask them to describe what was done in their own words
2. "What do you want to do next?" — ask them to state their intended next step(s)

Wait for their response. Use their answers to anchor the session summary and next steps.
Do not proceed to Step 1 until the user has replied.

## Step 1 — Gather current state
1. Inspect data/raw/ and data/processed/ to get the current pipeline state for all 6 quasars
2. Reconcile what you observed in the session with what the user described in Step 0
3. List any open issues or blockers discovered this session
4. Write numbered next steps that lead with what the user said they want to do next

## Step 2 — Write to sessions/<DATE>_session.md
Create a new file at sessions/<DATE>_session.md with full narrative detail:

### Session — <DATE>
**What we did:**
- ...

**Decisions made:**
- ...

**Open issues at end of session:**
- ...

This file is the permanent record — write it with enough detail that a future agent
reading it months from now would understand what happened and why.

## Step 3 — Overwrite HANDOFF.md with current state snapshot
HANDOFF.md is NOT a log — it is always the current state only. Overwrite it entirely:

# Project Handoff — Current State

> This file is always the current snapshot. Full session history is in `sessions/`.
> Read the most recent file there for narrative context, or look back further if needed.

## Last updated — <DATE>

**Pipeline state:**
| Quasar | Downloaded | Aligned | Drizzled |
|--------|-----------|---------|----------|
...

**Open issues:**
- ...

**Next steps:**
1. ...

## Approval
Show the proposed sessions/ entry and the proposed HANDOFF.md together before writing either.
Write both only after the user explicitly approves.

## Step 4 — Offer to push to GitHub
After writing both files, ask:
> "Would you like me to push these changes to GitHub?"

If yes: run the /push-to-github flow — show staged files and commit message for approval
before committing or pushing anything.
