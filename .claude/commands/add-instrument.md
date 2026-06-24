# /add-instrument
# Scaffolds support for a new instrument (e.g. ACS) alongside the existing WFC3/IR
# pipeline. Creates new notebooks, config, and processed subdirs. Shows full plan first.

Scaffold an extension of this project to support a new instrument or detector.

To use this command, specify:
- The instrument name (e.g. ACS, WFC3/UVIS)
- The detector or channel if relevant (e.g. ACS/WFC)
- Any known differences in processing compared to the WFC3/IR pipeline

IMPORTANT — Download notebook:
The download parameters for a new instrument will likely differ from the existing
WFC3/IR setup. The number of targets, file locations, MAST query criteria, file
types (e.g. FLC instead of FLT), and download process may all be different.
Do NOT simply copy 01_download_data.ipynb — instead, ask the user to describe
the new download requirements and build a fresh download notebook from scratch
tailored to the new instrument. Confirm all query parameters with the user
before writing any download code.

Steps the agent will propose (for approval before creating anything):
1. Ask the user about download requirements for the new instrument before assuming
   anything carries over from the WFC3/IR pipeline
2. Create a fresh download notebook for the new instrument with confirmed parameters
3. Add instrument-specific subdirs under data/processed/<quasar>/ for each quasar
   (e.g. aligned_acs/, drizzled_acs/)
4. Create a new config file: config/<instrument>_drizzle_params.yaml
5. Create adapted processing notebooks for alignment and drizzling
6. Update CLAUDE.md to document the new instrument and its differences
7. Update docs/pipeline_overview.md and docs/data_types.md

Show the full proposed file list and changes for approval before creating anything.
