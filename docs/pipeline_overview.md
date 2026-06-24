# Pipeline Overview — Glikman Quasars WFC3/IR

## Goal
Produce science-ready drizzle-combined mosaics for 6 quasars from HST proposal 14706
(PI: Glikman) using WFC3/IR imaging. The pipeline takes raw FLT files from MAST through
geometric alignment and AstroDrizzle combination.

## Step 1 — Download Data (`01_download_data.ipynb`)
**Input:** MAST archive query (proposal 14706, PI Glikman, WFC3/IR, project=HST)
**Output:** FLT files in `data/raw/<quasar>/`

Queries MAST using astroquery to retrieve all WFC3/IR FLT exposures from proposal 14706.
The query must use `project='HST'` (not 'HAP') to return individual calibrated exposures
rather than processed mosaics. Confirms exactly 6 distinct targets before downloading.

## Step 2 — Inspect Raw Data (`02_inspect_raw.ipynb`)
**Input:** FLT files in `data/raw/<quasar>/`
**Output:** Diagnostic plots and header summary

Examines FITS headers for all downloaded files. Plots raw images to check for obvious
artifacts, cosmic rays, or data quality issues before committing to processing.
Reports exposure times, filters, and image statistics per quasar.

## Step 3 — Align Images (`03_align_images.ipynb`)
**Input:** FLT files from `data/raw/<quasar>/`
**Output:** Alignment-corrected FLT files in `data/processed/<quasar>/aligned/`

Runs TweakReg to compute a refined astrometric solution across all exposures for each
quasar. The updated WCS is written back into copies of the FLT files saved to the
aligned/ directory. The originals in raw/ are never modified.

## Step 4 — Drizzle Combine (`04_drizzle.ipynb`)
**Input:** Aligned FLT files from `data/processed/<quasar>/aligned/`
**Output:** DRZ mosaic in `data/processed/<quasar>/drizzled/`

Runs AstroDrizzle on the aligned exposures for each quasar using parameters from
`config/wfc3_ir_drizzle_params.yaml`. Combines all exposures into a single
geometrically corrected, cosmic-ray rejected mosaic.

## Step 5 — Inspect Outputs (`05_inspect_outputs.ipynb`)
**Input:** DRZ files from `data/processed/<quasar>/drizzled/`
**Output:** Diagnostic plots and results summary

Reviews the final drizzled products for each quasar. Checks image quality, pixel
scale, astrometry, and signal-to-noise. Flags any quasars that may need parameter
adjustments and re-drizzling.

## Future Extension
The pipeline will be extended to ACS optical data. Use `/add-instrument` to scaffold
that extension when ready — note that ACS uses FLC files and a different calibration
pipeline than WFC3/IR. The download parameters will need to be confirmed fresh.
