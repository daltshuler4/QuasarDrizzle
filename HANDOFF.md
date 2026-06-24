# Project Handoff — Current State

> This file is always the current snapshot. Full session history is in `sessions/`.
> Read the most recent file there for narrative context, or look back further if needed.

## Last updated — 2026-06-24 (end-session)

**Progress since last update:**
- Completed notebook 01_download_data.ipynb — MAST query, HAP filtering, download, validation
- 80 FLT files downloaded across 6 quasars, verified 100% against MAST product list
- Files organized into data/raw/<TARGNAME>/ using FITS TARGNAME header directly

**Pipeline state:**
| Quasar | Downloaded | Aligned | Drizzled |
|--------|-----------|---------|----------|
| W2M0811+0115 | ✓ (16 FLT) | ✗ | ✗ |
| W2M1042+1641 | ✓ (16 FLT) | ✗ | ✗ |
| W2M1220+1126 | ✓ (8 FLT)  | ✗ | ✗ |
| W2M1252+0715 | ✓ (16 FLT) | ✗ | ✗ |
| W2M1439+0858 | ✓ (8 FLT)  | ✗ | ✗ |
| W2M1542+1259 | ✓ (16 FLT) | ✗ | ✗ |

W2M1220 and W2M1439 have 8 files (not 16) — confirmed intentional: 4-point dither vs
8-point for others. All quasars have 2 filters each (F160W + F105W or F125W).

**Open issues:**
- data/processed/ has empty quasar_01–06 placeholder folders — delete before notebook 03
- notebooks/mastDownload/ staging folder still on disk — safe to delete
- cell-11 target_name display shows UNKNOWN (cosmetic; use parent_obsid for join — no
  impact on actual data)
- Stop hook uses absolute path — will break on other machines

**Next steps:**
1. Delete empty data/processed/quasar_01–06 folders
2. Delete notebooks/mastDownload/
3. Open notebooks/02_inspect_raw.ipynb — examine raw FLT images, check exposure times
   and image quality before committing to alignment
4. After 02, proceed to notebook 03 (TweakReg alignment)
