# Issue

**Title:** Add energy-diff and dissipation columns to gather_results

## Description

This pull request enhances the `gather_results` function in `calphy.postprocessing` by adding new quality metrics and diagnostics to its output DataFrame, and introduces comprehensive tests for these additions. The main improvements include support for free energy error, dissipation, and detailed per-replica diagnostics, as well as robust handling of missing files and calculation modes.

**Enhancements to result metrics:**
- The DataFrame returned by `gather_results` now includes new columns: `free_energy_error`, `dissipation`, `ts_dissipation`, `forward_energy_diff`, `backward_energy_diff`, `forward_lambda`, and `backward_lambda`, with clear documentation for each.
- These columns are initialized and set to appropriate defaults (e.g., `np.nan` or `None`) to ensure consistent output even when data is missing. [[1]](diffhunk://#diff-bc9244e80bd86faa1a1c8a24943a3a7933046c3eb300e40fc15ec9070962cf6aR94-R107) [[2]](diffhunk://#diff-bc9244e80bd86faa1a1c8a24943a3a7933046c3eb300e40fc15ec9070962cf6aR142-R148)

**Parsing and diagnostics improvements:**
- For temperature sweep (`ts`) mode, the function now parses the statistical error from `temperature_sweep.dat` and extracts per-replica forward/backward energy differentials and lambda arrays from `ts.forward_i.dat`/`ts.backward_i.dat` files, providing detailed diagnostics for phase transitions.
- For other calculation modes, relevant metrics are set to fixed or default values as appropriate.

**Testing:**
- A new test suite in `tests/test_postprocessing.py` builds minimal calculation folders and validates the new metrics and fallback behaviors for different calculation modes and missing files.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3e65c630f4be`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.