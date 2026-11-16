# Issue

**Title:** Refresh the float box after the second half of the integration step

## Description

## Summary

The npt_ber, npt_scr, pimd and pimd_scr barostats change the box in `Integrate::compute2`. `Force::compute` refreshes the float copy of the box (`Box::float_h`, `Box::is_orthogonal`) only at the next step. The evaluations in `end_of_step` therefore used the float box of the previous step. This affects `dump_observer`, `active`, `dump_dipole`, `dump_polarizability`, `compute_extrapolation` and `compute_es`. The error shows when the small-box path of NEP visits more than one image in any direction, or on the large-box path. On rattled diamond with `nep_C.txt` and `npt_ber`, the largest force difference between `observer1.xyz` and a single-point evaluation of the same frame is 1.7e-3 eV/Å for 64 atoms and 3.0e-2 eV/Å for 1728 atoms. The MTTK ensembles and MSST change the box in `compute1`, and `Force::compute` refreshes it in the same step. They are not affected.

This PR adds a call to `Box::set_is_orthogonal` after the ensemble step in `Integrate::compute2`. The largest force difference is reduced to 3.9e-6 eV/Å for 64 atoms and 1.2e-5 eV/Å for 1728 atoms, the level of an `nve` run. `dump_observer` in observe mode and `active` pass the forces of the main potential on to the next half-kick. With these barostats this PR therefore also changes their trajectories.

## Testing

`tests_pytest/test_dump_observer.py` runs `dump_observer observe` with two copies of `nep_C.txt` for 5 steps, with `nve` and `npt_ber`, on a 64-atom cell (small-box path with two images per direction) and a 1728-atom cell (large-box path). It compares the forces of the last frame of `observer1.xyz` with a single-point evaluation of that frame. Both `npt_ber` cases fail on master and pass with the fix.

`pytest -m "not slow"` in `tests_pytest`: 655 passed, 478 skipped. `clang-format` makes no changes, and `flake8` is clean on the new test.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c08b187c80ef`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.