# /resume-editing
# Switches this agent from advisory mode to full editing mode.
# Before accepting editing responsibility, reads HANDOFF.md and checks whether
# unsaved work exists from another session or window.

Transition from advisory mode to full editing mode safely.

## Step 1 — Read HANDOFF.md
Read HANDOFF.md and summarize the current pipeline state in 2-3 sentences.
Required before making any edits, regardless of what was discussed in advisory mode.

## Step 2 — Check for unsaved work
Compare two timestamps:
- The timestamp in `.claude/session_heartbeat.txt` (last AI activity)
- The date of the most recent session entry in HANDOFF.md (last saved state)

**If the heartbeat is newer than the last HANDOFF.md entry**, unsaved work exists.
This means one of two things:
- A previous session ended without /end-session (crash or accidental close)
- Another Claude Code window is currently active and has not yet checkpointed

Respond with:
> "The session heartbeat is newer than the last HANDOFF.md entry, which means
> there is unsaved work from a previous or currently active session.
>
> This could be a crash/close from last time, or another window that is still
> actively working on this project.
>
> Before I switch to editing mode, please:
> - If you had a previous session that crashed: run /reconstruct-session first
> - If another window is currently open and working: ask it to run /checkpoint,
>   then close it or switch it to advisory mode before continuing here"

Wait for the user to confirm it is safe before proceeding.

**If the heartbeat matches or is older than the last HANDOFF.md entry**, the state
is clean — no unsaved work detected. Proceed to Step 3.

## Step 3 — Confirm and switch
> "HANDOFF.md is current, no unsaved work detected. Switching to editing mode."

From this point on, operate as a full editing agent.
