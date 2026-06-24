# Session Handoff Log

## Session — 2026-06-24
**What we did:**
- Designed the full project structure with AI-friendly organization
- Created directory skeleton: notebooks/, docs/, config/, data/raw/, data/processed/
- Created CLAUDE.md, HANDOFF.md, 12 command files, doc stubs, and config stub
- Data directories use quasar_01–06 placeholders pending the MAST query
- Created AI_PROJECT_SETUP_TEMPLATE.md at repo root for reuse on future projects

**Pipeline state:**
| Quasar | Downloaded | Aligned | Drizzled |
|--------|-----------|---------|----------|
| quasar_01 | ✗ | ✗ | ✗ |
| quasar_02 | ✗ | ✗ | ✗ |
| quasar_03 | ✗ | ✗ | ✗ |
| quasar_04 | ✗ | ✗ | ✗ |
| quasar_05 | ✗ | ✗ | ✗ |
| quasar_06 | ✗ | ✗ | ✗ |

**Open issues:**
- Quasar folder names are placeholders — run /rename-quasars after notebook 01 confirms
  the 6 target names from MAST

**Next steps:**
1. Open `notebooks/01_download_data.ipynb`
2. Run the MAST query for proposal 14706, PI Glikman, instrument WFC3/IR, project HST
3. Confirm exactly 6 distinct targets are returned before downloading
4. Download FLT files to data/raw/<quasar>/
5. Run /rename-quasars to replace placeholder folder names with real target names
