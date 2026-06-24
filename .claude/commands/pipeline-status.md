# /pipeline-status
# Checks which quasars have been downloaded, aligned, and drizzled.
# Produces a status table showing current progress across all 6 quasars.

Inspect the data/ directory to report the current pipeline state for all quasars:

For each folder in data/raw/:
- Is it non-empty? How many .fits files does it contain?

For each folder in data/processed/:
- Does aligned/ contain files?
- Does drizzled/ contain files?

Produce a status table:

| Quasar | Raw FLT Files | Aligned | Drizzled |
|--------|--------------|---------|----------|
| ...    | ...          | ✓/✗     | ✓/✗      |

Then note:
- Any quasars with zero files at any stage (possible download failure)
- Any quasars where file counts differ significantly from the others
- The recommended next step based on current state
