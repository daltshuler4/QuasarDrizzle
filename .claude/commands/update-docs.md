# /update-docs
# Refreshes docs/quasars.md and docs/pipeline_overview.md to reflect the current
# state of the project. Shows proposed changes for approval before writing.

Refresh the documentation files in docs/:

1. **docs/quasars.md** — update the target table based on:
   - What folders currently exist in data/raw/ and data/processed/
   - Any metadata readable from downloaded FITS headers (target name, RA, Dec,
     filter, number of exposures, total exposure time)
   - Current pipeline status for each quasar

2. **docs/pipeline_overview.md** — verify that the described workflow matches
   the actual notebooks:
   - Are all 5 notebooks described accurately?
   - Do the described inputs/outputs match what the notebooks actually do?
   - Is there anything in the notebooks that is not documented?

For each file, show the proposed changes as a clear before/after diff.
Ask for confirmation before writing to either file.
Do not make changes without explicit approval.
