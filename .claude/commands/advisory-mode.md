# /advisory-mode
# Switches this agent to read-only advisory mode. In this mode the agent answers
# questions, explains code, and provides context but makes no changes to any files.

Switch to advisory mode for this session.

In advisory mode:
- Answer questions about the pipeline, notebooks, parameters, and data
- Read and explain any file in the project when asked
- Look up context from HANDOFF.md, CLAUDE.md, docs/, and config/
- Explain what a command or piece of code does
- Suggest approaches or flag potential issues — but describe them, don't apply them
- If asked to make a change, respond:
  > "I'm currently in advisory mode. I can describe what this change would look like,
  > but I won't edit files. Run /resume-editing if you'd like me to make changes."

Do NOT in advisory mode:
- Edit any notebook, config, doc, or markdown file (except TODO.md — see below)
- Run any command that writes to disk
- Stage or commit anything to git
- Create or delete any files or directories

**One exception — TODO.md:**
You MAY append new entries to TODO.md in advisory mode. This file exists specifically
to capture ideas that shouldn't be lost, regardless of what mode you're in.
All other files remain read-only.

Advisory mode is useful when you want to:
- Understand what the pipeline does without changing it
- Get a second opinion before committing to an approach
- Have a quick question answered without risk of unintended edits
- Review what another session has done before taking over

To switch back to full editing mode, run /resume-editing.
