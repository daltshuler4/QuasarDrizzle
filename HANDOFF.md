# Project Handoff — Current State

> This file is always the current snapshot. Full session history is in `sessions/`.
> Read the most recent file there for narrative context, or look back further if needed.

## Last updated — 2026-06-29

**Progress since last update:**
- ACS pipeline scaffolded (notebooks, data_acs/ folder, config sections) but revealed
  to need a clean rebuild — copied WFC3 structure introduced too many hidden assumptions
- ACS data downloaded: 5 quasars confirmed, mix of FLC and FLT files discovered
- W2M2152-0051 found to be all FLT (16 files, no CTE correction applied) — new target
- Multiple notebook bugs fixed: glob suffix, YAML indent, cwd crash vulnerability

**Pipeline state — WFC3/IR (`data/`):**
| Quasar | Downloaded | Aligned | Drizzled |
|--------|-----------|---------|----------|
| W2M0811+0115 | ✓ (16 FLT) | ✗ | ⚠ unverified (F105W, F160W) |
| W2M1042+1641 | ✓ (16 FLT) | ⚠ stale | ⚠ unverified — dropped from pipeline |
| W2M1220+1126 | ✓ (8 FLT) | ✗ | ⚠ unverified (F105W, F160W) |
| W2M1252+0715 | ✓ (16 FLT) | ✗ | ⚠ unverified (F105W, F160W) |
| W2M1439+0858 | ✓ (8 FLT) | ✗ | ⚠ unverified (F105W, F160W) |
| W2M1542+1259 | ✓ (16 FLT) | ✗ | ⚠ unverified (F125W, F160W) |

**Pipeline state — ACS/WFC (`data_acs/`):**
| Quasar | FLC | FLT | Drizzled |
|--------|-----|-----|---------|
| W2M0035+0114 | 8 | 8 | ⚠ unverified (F475W + F814W _drc) |
| W2M0043+0052 | 4 | 12 | ⚠ F475W _drc only — F814W needs CTE correction |
| W2M1106+0221 | 4 | 12 | ⚠ F475W _drc only — F814W needs CTE correction |
| W2M1242+0440 | 8 | 8 | ⚠ unverified (F475W + F814W _drc) |
| W2M2152-0051 | 0 | 16 | ✗ — all FLT, CTE correction required first |

**Open issues:**
- ACS target folders still exist in data/raw/ alongside WFC3 data — needs cleanup
- WFC3: updatehdr=False — Gaia corrections not written to any FLT headers
- WFC3: all drizzled outputs unverified
- ACS: W2M2152-0051 and FLT exposures for W2M0043+0052, W2M1106+0221 need CTE correction
- ACS: current notebooks adapted from WFC3 copy — earmarked for clean rebuild

**Next steps:**
⭐ PRIMARY: Clean rebuild of ACS pipeline from scratch, cell-by-cell alongside the agent.
   Also clean up file structure (remove ACS folders from data/raw/).

1. Clean up data/raw/ — remove ACS quasar folders that were left behind
2. Rebuild ACS notebooks from scratch (user provides task per cell, agent builds clean)
3. Download RAW ACS files and run CTE correction (CalACS) on FLT-only targets
4. Add `target_name` list to ACS download query (W2M0035+0114, W2M0043+0052,
   W2M1106+0221, W2M1242+0440, W2M2152-0051) in config under `acs_download`
5. Verify WFC3 drizzled output quality for all 5 active targets
6. Decide whether to rerun WFC3 TweakReg with updatehdr=True before re-drizzling
