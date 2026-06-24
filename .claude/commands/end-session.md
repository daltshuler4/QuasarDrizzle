# /end-session
# Writes a structured end-of-session update to HANDOFF.md so the next agent
# starts with full context. Always shows the proposed update for approval first.

Write a structured end-of-session update to HANDOFF.md:

1. Summarize what was accomplished this session in 3–5 specific bullet points
   (be concrete — name the notebooks run, quasars processed, issues resolved)
2. Update the pipeline state table for all 6 quasars based on the current
   contents of data/raw/ and data/processed/
3. List any open issues or blockers discovered this session
4. Write numbered next steps that are specific and immediately actionable —
   the next agent should be able to read these and know exactly what to do
5. Use today's date as the session header

Format:
## Session — <DATE>
**What we did:**
- ...

**Pipeline state:**
| Quasar | Downloaded | Aligned | Drizzled |
...

**Open issues:**
- ...

**Next steps:**
1. ...

Prepend this as a new section at the TOP of HANDOFF.md so the most recent
session is always first. Show the full proposed update for approval before writing.
