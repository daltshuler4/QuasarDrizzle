# /push-to-github
# Stages relevant files, writes a commit message, and pushes to the current branch.
# Always shows a diff summary and asks for approval before committing.

Prepare and push current work to GitHub:

1. Run `git status` to show what has changed since the last commit
2. Identify files to stage — include notebooks, config, docs, command files, and
   CLAUDE.md/HANDOFF.md. Exclude data/ and any .fits, .log, or other large files.
3. Review HANDOFF.md to understand what was accomplished this session and use that
   context to draft a clear, specific commit message
4. Show the user:
   - The list of files to be staged
   - The proposed commit message
   - A brief summary of what changed
5. Wait for explicit approval before proceeding
6. After approval: stage the files, commit, and push to the current branch

Never push data files or .fits files.
Never force push.
Never commit without showing the user the diff summary first.
