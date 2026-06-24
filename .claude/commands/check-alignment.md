# /check-alignment
# Parses TweakReg logs in data/processed/*/aligned/ for errors, warnings,
# and large residuals. Flags any quasars that may need re-alignment.

Review TweakReg alignment results across all quasars:

1. Look for any log files in data/processed/*/aligned/ (typically tweakreg.log
   or similar) and scan for ERROR or WARNING entries
2. Check for large RMS residuals — flag any quasar where the reported fit RMS
   is significantly higher than the others (may indicate poor alignment)
3. Check that the expected number of aligned FLT files exists in each
   data/processed/<quasar>/aligned/ directory
4. Note any quasars where TweakReg failed to find enough sources for a reliable fit
5. Note any quasars where the alignment solution was rejected or fell back to
   a default WCS

Produce a per-quasar alignment summary:
| Quasar | Aligned Files | RMS Residual | Issues |
|--------|--------------|--------------|--------|

Recommend whether it is safe to proceed to AstroDrizzle or whether any
quasars need re-alignment before continuing.
