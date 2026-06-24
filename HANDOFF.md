# Project Handoff — Current State

> This file is always the current snapshot. Full session history is in `sessions/`.
> Read the most recent file there for narrative context, or look back further if needed.

## Last updated — 2026-06-24

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
- Quasar folder names are placeholders — run /rename-quasars after notebook 01
- environment.yml not yet created
- Stop hook uses absolute path — will break on other machines
- hst_notebooks submodule added; old sibling repo has 2 unpushed commits (low priority)

**Next steps:**
1. Create environment.yml with drizzlepac, astroquery, astropy, matplotlib, pyyaml
2. Document Stop hook path fragility in README.md Known Issues
3. Open notebooks/01_download_data.ipynb and run MAST query for proposal 14706, project=HST
4. Confirm exactly 6 targets before downloading anything
5. Run /rename-quasars after download to replace quasar_01–06 with real target names
