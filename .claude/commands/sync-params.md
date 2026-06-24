# /sync-params
# Checks that config/wfc3_ir_drizzle_params.yaml matches what is actually used
# in 04_drizzle.ipynb. Flags drift between config and notebook. Reports only.

Check consistency between the config file and the drizzle notebook:

1. Extract all parameter values from config/wfc3_ir_drizzle_params.yaml
2. Open notebooks/04_drizzle.ipynb and search for:
   - Any hardcoded AstroDrizzle parameters (pixfrac, kernel, final_scale,
     final_bits, driz_sep_bits, combine_type, etc.)
   - Any parameters read from the config file
3. Flag parameters that appear hardcoded in the notebook but also exist in config
   (these should be read from config instead)
4. Flag parameters that are in config but never referenced in the notebook
   (these may be stale or forgotten)
5. Flag parameters in the notebook that are not in config at all
   (these should be added to config for transparency)

Report discrepancies clearly. Do not modify any files — the user decides
how to resolve each discrepancy.
