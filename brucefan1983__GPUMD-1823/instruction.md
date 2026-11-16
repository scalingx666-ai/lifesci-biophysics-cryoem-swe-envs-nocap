# Issue

**Title:** Derive the default PPPM spacing of qNEP models from the radial cutoff

## Description

## Summary

For qNEP models, `NEP_Charge` sets the Ewald splitting parameter to alpha = pi / rc_radial, while the default PPPM mesh spacing is a fixed 1 Å. The reciprocal-space part grows with alpha, and for a short radial cutoff the 1 Å mesh does not resolve it. GPUMD also prints the mesh without any estimate of its error.

This MR sets the default spacing to min(1, rc_radial / 6) Å in `NEP_Charge::check_ewald_pppm`. The default keeps the product of alpha and the target spacing at or below pi / 6, which is its value at rc_radial = 6 Å. Models with rc_radial of 6 Å or more keep the 1 Å mesh. An explicit `kspace pppm <spacing>` takes precedence over the default.

This MR adds an estimate of the root-mean-square PPPM force error, which `PPPM::print_mesh` prints after the mesh line at the first force evaluation. It is Eq. (38) of Deserno and Holm, J. Chem. Phys. 109, 7694 (1998), for ik differentiation and five-point charge assignment, with the coefficients of Table II therein and the charges of that step. Eq. (38) holds for a cubic box. `PPPM::print_mesh` evaluates it per direction with the thickness of the cell as the box length and reports the root mean square of the three values. `tests_regression/pppm_checks.py` accepts the new line.

`doc/gpumd/input_parameters/kspace.rst` states the default and the estimate, and states that a `kspace` line applies to every `run` wherever it appears in `run.in`.

This MR changes the PPPM energies and forces of models with rc_radial below 6 Å. For the rc_radial = 4 Å water model in `tests_pytest/fixtures/models/qnep_mode1_water.txt` on a 1701-atom cell, the mesh rises from 28 x 40 x 20 to 40 x 56 x 28. The PPPM energy minus the Ewald energy is reduced from -0.353 to -0.0016 meV/atom, and the RMS force difference per component is reduced from 2.25 to 0.19 meV/Å. The speed is 2.51e6 atom*step/s against 2.53e6 with the 1 Å mesh, corresponding to about 1 percent. The finer mesh reaches the limit of 512^3 points in smaller boxes: an rc_radial = 4 Å model stops with "PPPM mesh is too large" in a cubic box of 345 Å, which runs with `kspace pppm 1.0`.

On the same water cell, the ratio of the estimate to the RMS force difference between PPPM and Ewald is 0.97, 1.13 and 1.20 at spacings of 0.5, 0.7 and 1.0 Å. For the mode-2 water model it is between 1.27 and 1.53 at spacings of 0.7 to 1.5 Å. At 0.5 Å it is 0.28, where the measured difference is 2.6e-5 eV/Å. The estimate assumes randomly distributed charges. For a 1080-atom BaTiO3 crystal it exceeds the measured difference by factors of 1.8 to 31, which the documentation states.

## Testing

`test_pppm_spacing` in `tests_pytest/test_kspace_consistency.py` reads the target spacing from the PPPM mesh line of `gpumd`. It covers a model with rc_radial = 4 Å without a `kspace` line, models with rc_radial = 4, 6 and 8 Å with `kspace pppm`, and `kspace pppm 0.9` before and after `run`. The rc_radial = 4 Å cases fail on master, and the rc_radial = 8 Å case fails with the uncapped rc_radial / 6.

`test_pppm_force_error_estimate` in the same file compares the printed estimate with the RMS force difference between PPPM and Ewald on the 1701-atom water cell at 0.5, 0.7 and 1.0 Å, within 25 percent. `test_force_error_estimate` in `tests_regression/test_pppm_checks.py` checks that the checker accepts one estimate line after the mesh line and rejects a misplaced, repeated or negative one.

- `pytest -m "not slow"` in `tests_pytest`: 671 passed, 478 skipped.
- `run_regression.py --suite pppm` with a `-DDEBUG` build of this branch on both sides: 37 passed, 0 failed, relations 2 passed, 23 skipped.
- `python -m unittest test_runner test_manifest test_post_checks test_pppm_checks` in `tests_regression`: 27 tests OK.
- `sphinx-build -b html doc`: 0 warnings.
- `git clang-format` against master: no changes.

A run of `run_regression.py` with master as the baseline shows the new estimate line as a stdout difference in the PPPM cases.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6811bc05455f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.