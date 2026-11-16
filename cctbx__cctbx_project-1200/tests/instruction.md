# Issue

**Title:** Kinemage: add suitealyze (RNA suite) markup to kinemage output

## Description

## Summary

Final piece of the kinemage refactor series: thread the suitealyze (RNA suite)
result through the kinemage builders so RNA suite outliers appear in kinemage output.

`suite_result` is now a parameter on `_build_kinemage`, `build_kinemage_from_model`,
`make_multikin`, and `export_molprobity_result_as_kinemage`. When a model contains RNA,
each builder computes (or accepts a pre-run) `suitealyze` result and emits its
`as_kinemage_markup()` — a `@subgroup {suites}` with suite labels and gold outlier
vectors. Protein-only models are unaffected (the block is only added when RNA is present).

All dependencies are already on master: the `suitealyze` class and its
`as_kinemage_markup()` (`mmtbx/suitename/suitealyze.py`), `rna_validation.suites` used by
the molprobity export path, and `build_kinemage_from_model` (added earlier in this series).

### One fix on top of the cherry-pick

`suitealyze.as_kinemage_markup()` returns a block that is **not** newline-terminated, so
appending it directly (`kin_out += suite_result.as_kinemage_markup()`) glued the following
section's `@subgroup` header onto the last suite point's coordinate line — malformed
kinemage on RNA models (e.g. `...9.057@subgroup {ribbon A NA}`). Added a trailing `"\n"` at
the append site in `_build_kinemage` so the next section starts on its own line. One line,
localized to the kinemage builder.

## Testing

- `mmtbx/kinemage/tst_validation.py`: all pass.
- phenix_regression `validation/tst_kinemage.py`: OK.
- `phenix.kinemage` **and** `build_kinemage_from_model` on an RNA model (404D): the
  `@subgroup {suites}` / `@labellist {suitelabels}` / `@vectorlist {suites}` markup now
  renders; a before/after diff against master shows the suite block is the only added
  content, and the `@subgroup` headers that follow it are correctly on their own lines.
- `phenix.kinemage` on a protein (1nxbH): 3481 greentint probe dots unchanged, no suite
  section emitted (no RNA), no malformed lines.
- `libtbx.find_clutter` and `py_compile` clean.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ac16644478a1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.