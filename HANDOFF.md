# Project Handoff — Current State

> This file is always the current snapshot. Full session history is in `sessions/`.
> Read the most recent file there for narrative context, or look back further if needed.

## Last updated — 2026-07-01

**Progress since last update:**
- ACS CTE correction completed for all subarray targets
- ACS drizzle notebook rebuilt from scratch; all 10 DRC files produced
- ACS pipeline now fully processed through drizzle for all 5 targets

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
| Quasar | FLC | FLT | DRC (F475W) | DRC (F814W) |
|--------|-----|-----|-------------|-------------|
| W2M0035+0114 | 8 | 8 | ✓ full-frame (~9974×9974) | ✓ full-frame (~9973×9973) |
| W2M0043+0052 | 12 | 12 | ✓ full-frame (~7915×7949) | ✓ subarray (~3984×3911) |
| W2M1106+0221 | 12 | 12 | ✓ full-frame (~9307×9331) | ✓ subarray (~4659×4617) |
| W2M1242+0440 | 8 | 8 | ✓ full-frame (~9972×9974) | ✓ full-frame (~9972×9974) |
| W2M2152-0051 | 16 | 16 | ✓ subarray (~4835×4807) | ✓ subarray (~4835×4807) |

**Open issues:**
- Intermediate drizzle files (_blt, _crmask, etc.) remain in data_acs/raw/ from last
  run; cleanup cell in 03_drizzle_acs.ipynb will remove them on next run
- ACS DRC outputs not yet visually inspected (inspect cell not run this session)
- WFC3 drizzled outputs remain unverified
- WFC3 alignment: updatehdr=False — Gaia corrections not written to FLT headers

**Next steps:**
1. ⭐ PRIMARY: PSF construction for ACS DRC mosaics
2. Run inspect cell in 03_drizzle_acs.ipynb to visually verify all 10 DRC outputs
3. Verify WFC3 drizzled output quality
