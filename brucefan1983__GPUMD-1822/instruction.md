# Issue

**Title:** Leave the run unchanged with active and report its uncertainty correctly

## Description

## Summary

`active` evaluates every potential into arrays of its own through `Force::compute_one_potential`, the dispatch that the run uses, and writes the frames of `active.xyz` from the arrays of the main potential. The per-atom arrays and the thermo vector of the run, and the host copies of the positions and velocities in `Atom`, are left as they are. The uncertainty of an atom is the norm of the standard deviations of its force components over the M models with the factor 1/M, accumulated with Welford's update, and `active.rst` writes the estimator out. A NaN uncertainty on any atom is the maximum and writes the frame whatever the threshold. `active` requires at least two potentials, and it leaves the multiple-potentials mode to `dump_observer`. The frame writer is one function that copies the frame to the host and then prints it. The documentation of `active`, `active.xyz` and `active.out` defines the main potential, states which potential propagates the run, and describes the header fields and columns of a frame with their units.

This PR changes the following behavior. The trajectory no longer depends on `active`: on master, `active` replaces the forces of the step with the bare forces of the main potential at every check, which drops the forces of `add_force`, `add_efield`, `add_spring`, `plumed` and the other `post_force` actions, and with `add_force` on a 64-atom diamond cell the temperature after 50 steps is 956.58 K with `active` against 957.11 K without it. A temperature-dependent NEP is evaluated at the temperature of the step. With `dump_observer average`, the average of the potentials propagates the run whichever keyword comes first. Two identical models give an uncertainty of 0 instead of 3.5e-8 to 6.2e-8 eV/Å. A velocity keyword in a later run draws the same velocities with and without `active`. The energy, virial and stress of a frame belong to the written positions and velocities, which under barostats, velocity rescaling and PIMD differ from those of the thermo vector of the run.

## Breaking changes

`active` with fewer than two potentials stops with the input error "active requires at least two potentials." A single potential gives an uncertainty of 0 at every step. `Force` accepts several potentials only when all are NEP potentials, so two or more potentials also mean that every potential is a NEP potential.

## Testing

`tests_pytest/test_active.py` adds nine tests. They require `thermo.out` to be identical with and without `active` (with `add_force`, in a later run that starts with `velocity`, under a temperature ramp with a temperature-dependent NEP, and with `dump_observer average` in both orders). They check the energy, forces, virial and stress of frames against single points of the main potential, the per-atom uncertainty of five checks against the 1/M estimator from single points of both models, zero uncertainty for identical models, a NaN uncertainty on all but the first atom, and the input error for one potential. The forces of a frame agree with the single point within 5e-5 eV/Å, which covers the fp32 rounding differences of up to 1.2e-5 eV/Å between the evaluation in the run and the single point. Fifteen mutants of `active.cu` and `force.cu` each make at least one of the tests fail.

`pytest -m "not slow"` in `tests_pytest` gives 671 passed, 478 skipped and 0 failed. In `tests_regression`, the 231 cases outside the `training` suite, run with master `f73e92cc` as the baseline, give 211 passed and 20 failed. Fifteen of the failures also occur in repeated runs of master against itself. The other five, `active_no_threshold`, `active_interval_10`, `active_low_threshold`, `active_no_fields` and `active_without_observer`, differ only in the `stress` field of `active.xyz`, by at most 4.61e-6 eV/Å³, which is the kinetic term of the velocities after the Berendsen rescale of the step. `active.out` is identical in all five. The `training` suite was not run, since the PR does not touch `src/main_nep`. `clang-format` leaves the changed lines unchanged, and `flake8` reports nothing on the test file.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9bbaed439334`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.