# /summarize-results
# Generates a science results summary table across all 6 quasars once drizzling
# is complete. Writes the table to docs/quasars.md under a Results section.

Generate a results summary across all quasars that have final output in
data/processed/<quasar>/drizzled/:

For each completed quasar, report:
- Target name
- Filter(s) used
- Number of input FLT exposures combined
- Total exposure time (seconds)
- Output pixel scale (arcsec/pixel)
- Output image dimensions (pixels)
- Any notable issues from alignment or drizzle logs

Produce a summary table:
| Quasar | Filter | N Exp | Total Exp Time | Pixel Scale | Dimensions | Notes |
|--------|--------|-------|---------------|-------------|------------|-------|

Also note any quasars that have not yet been drizzled.

Propose adding this table to docs/quasars.md under a "## Results" section.
Show the proposed addition for approval before writing.
