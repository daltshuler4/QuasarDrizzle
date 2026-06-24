# /notebook-check
# Scans all notebooks for redundant code, hardcoded parameters, and inconsistencies.
# Reports findings only — does not make changes.

Scan all notebooks in the notebooks/ directory for the following issues:

1. **Redundant code** — logic that appears in more than one notebook and could be
   shared or consolidated (e.g. repeated imports, repeated file path setup)
2. **Hardcoded parameters** — any values that should instead be read from
   config/wfc3_ir_drizzle_params.yaml (pixfrac, kernel, output scale, etc.)
3. **Inconsistent patterns** — variable names, file path construction, or quasar
   folder references that differ across notebooks without good reason
4. **Dead cells** — commented-out code blocks or cells with no output that serve
   no explanatory purpose
5. **Stale markdown** — section headers or explanations that no longer match what
   the code below them actually does

Report findings grouped by notebook, with the specific cell number or content
for each issue. Do not modify any notebooks — present the report so the user
can decide what to fix.
