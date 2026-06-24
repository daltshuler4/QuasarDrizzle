# /log-issue
# Appends a dated known issue entry to the "Known Issues" section of CLAUDE.md.
# This section is read by every agent at the start of every session.

Add a known issue to the "Known Issues — Always Check Before Acting" section of CLAUDE.md.

Ask the user:
1. What mistake or incorrect behavior occurred? (be specific — what did the agent do wrong?)
2. What is the correct behavior, or what should the agent check/avoid instead?
3. Does this apply always, or only in a specific context?

Then compose a clear, dated entry:
  - [ ] Issue logged <DATE>: <what to watch out for and why, in one or two sentences>

Show the proposed entry to the user for approval before writing.

After approval, append the entry to the Known Issues section in CLAUDE.md.
Do not modify any other part of CLAUDE.md.

The Known Issues section is the most important part of CLAUDE.md for preventing
repeated mistakes — keep entries specific and actionable.
