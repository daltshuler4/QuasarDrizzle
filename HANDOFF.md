# Project Handoff — Current State

> This file is always the current snapshot. Full session history is in `sessions/`.
> Read the most recent file there for narrative context, or look back further if needed.

## Last updated — 2026-06-29

**Progress since last update:**
- Notebook cleanup complete for 01 and 02: dead imports removed, all TweakReg
  parameters moved to config/wfc3_ir_drizzle_params.yaml under new `tweakreg:` section
- Confirmed alignment diagnosis for W2M1042+1641: 4 nan-shift images are GSC242
  (already on Gaia frame, correct behavior); 12 GSC240 images got valid shifts and
  are ready for updatehdr=True
- Root cause of drizzled doubling identified: Gaia DR1 (GSC240) vs DR2 (GSC242)
  frame offset — will be resolved once TweakReg is rerun with updatehdr=True
- /end-session skill updated to ask user for session account before writing

**Pipeline state:**
| Quasar | Downloaded | Aligned | Drizzled |
|--------|-----------|---------|----------|
| W2M0811+0115 | ✓ (16 FLT) | ✗ | ⚠ unverified (2 DRZ) |
| W2M1042+1641 | ✓ (16 FLT) | ⚠ stale copies in aligned/ | ⚠ unverified (2 DRZ) |
| W2M1220+1126 | ✓ (8 FLT) | ✗ | ⚠ unverified (2 DRZ) |
| W2M1252+0715 | ✓ (16 FLT) | ✗ | ⚠ unverified (2 DRZ) |
| W2M1439+0858 | ✓ (8 FLT) | ✗ | ⚠ unverified (2 DRZ) |
| W2M1542+1259 | ✓ (16 FLT) | ✗ | ⚠ unverified (2 DRZ) |

TweakReg diagnostic complete for W2M1042+1641. Gaia catalog, shift file,
residual/vector PNGs all in `data/tweakreg/`. 12/16 images fit; 4 nan shifts
(GSC242 files — correct, no correction needed).

**Open issues:**
- updatehdr=False — Gaia WCS corrections not yet written to any FLT headers
- aligned/ for W2M1042+1641 has stale FLTs — delete before rerunning with updatehdr=True
- 03_drizzle.ipynb imports cell is out of order (cell 3, must move to cell 1)
- 03_drizzle.ipynb: final_rot=0. hardcoded; skysub and clean in config but not wired up
- All 6 drizzled outputs from prior buggy run — quality unverified

**Next steps:**
1. Verify Gaia.MAIN_GAIA_TABLE in notebook 02 (confirm DR3, not DR2)
2. Rerun TweakReg with updatehdr=True for W2M1042+1641
3. Clear stale aligned/ for W2M1042+1641 and copy Gaia-corrected FLTs
4. Fix 03_drizzle.ipynb: move imports cell to position 1, add final_rot to config,
   wire up skysub and clean
5. Rerun drizzle for W2M1042+1641 and inspect output for clean single images
6. Verify drizzled output quality for all 6 quasars
