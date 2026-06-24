# /validate-downloads
# Verifies that all 6 quasars have the expected FLT files before processing begins.
# Reports any missing, empty, or suspicious files. Does not modify anything.

Verify the integrity of downloaded data in data/raw/:

1. Check that all 6 quasar folders exist and are non-empty
2. Confirm all quasars have a similar number of .fits files — flag any outlier
   (e.g. one quasar with far fewer files than the others)
3. Confirm all files have the .fits extension and are non-zero in size
4. If possible, open each file header and confirm:
   - INSTRUME = 'WFC3'
   - DETECTOR = 'IR'
   - PROPOSID = 14706
   - File type is FLT (FILETYPE keyword)
5. Report a per-quasar summary and flag anything that looks wrong

Do not modify any files. If issues are found, describe what corrective action
to take before proceeding to notebook 03.
