# HST File Types Reference

## File Types Used in This Pipeline

### FLT — Flat-fielded, calibrated exposure (WFC3/IR)
- **Produced by:** CALWF3
- **Used in:** Steps 1–3 (download, inspect, align)
- **Contents:** Single calibrated exposure with WCS header; not geometrically corrected
- **Units:** Electrons per second (e-/s)
- **Note:** This is the primary input file for TweakReg and AstroDrizzle

### DRZ — Drizzle-combined mosaic
- **Produced by:** AstroDrizzle
- **Used in:** Step 5 (inspect outputs)
- **Contents:** Multi-extension FITS with science (SCI), weight (WHT), and context (CTX) extensions
- **Units:** Electrons per second (e-/s), resampled to output pixel grid
- **Note:** Final science product for each quasar

---

## File Types NOT Used Here (for reference)

### FLC — Flat-fielded, CTE-corrected exposure (ACS / WFC3/UVIS only)
- WFC3/IR does not suffer from charge transfer efficiency (CTE) issues
- FLC files are used for ACS and WFC3/UVIS — they will be needed when this
  pipeline is extended to optical data via `/add-instrument`

### DRC — Drizzle-combined mosaic with CTE correction (ACS / WFC3/UVIS only)
- The ACS equivalent of DRZ; includes CTE correction in the combination

### RAW — Uncalibrated readout
- Not used; MAST delivers FLT files which are already calibrated

---

## FITS Extensions in a DRZ File

| Extension | Name | Contents |
|-----------|------|----------|
| [0] | PRIMARY | Header only; global metadata |
| [1] | SCI | Science image (flux in e-/s) |
| [2] | WHT | Weight image (inverse variance) |
| [3] | CTX | Context image (which inputs contributed to each pixel) |
