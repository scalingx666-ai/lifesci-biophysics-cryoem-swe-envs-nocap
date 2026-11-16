# Issue

**Title:** Evaluate the dump_observer potentials into scratch arrays

## Description

## Summary

In `observe` mode, `dump_observer` evaluated every potential into the per-atom arrays and the thermo vector of the run, with the main potential last. The first half kick of the next step then read the bare forces of the main potential, recomputed at the end of the step. Forces added in `post_force`, such as by `add_force`, `add_efield`, `add_spring`, the HNEMD driving force and `plumed`, were therefore dropped at every output step, and the trajectory depended on whether observers were written. The observers also called `Potential::compute` directly, which skips the temperature argument of a temperature-dependent NEP (`model_type 3`) and `compute_ilp` of the ILP potentials. A temperature-dependent NEP was observed 0.42 eV below the energy of the run, and an ILP observer wrote zeros.

This PR evaluates every potential, the main potential included, into scratch arrays held by `Dump_Observer`. The new `Force::compute_one_potential` zeroes the given arrays and evaluates one potential through the same dispatch as `Force::compute`. The per-atom arrays and the thermo vector of the run stay unchanged, and each observer file holds the energy, virial and forces of its potential alone at the written positions. The forces in `observer0.xyz` therefore exclude the forces that other keywords add, which `doc/gpumd/input_parameters/dump_observer.rst` now states. The documentation also states that the `stress` field of the observer frames is the pressure tensor, which adds the kinetic contribution of the velocities to the virial, and lists the file names `observer.out` and `observer.xyz` used with a single potential. The PR builds on the float-box refresh in `Integrate::compute2` from #1806.

## Testing

`tests_pytest/test_dump_observer.py` adds `test_observers_leave_run_unchanged`, which runs `add_force`, `npt_ber` and `pimd` with one and with two potentials, the second a scaled copy of `nep_C.txt`. It requires `thermo.out` to be identical with and without the observer, with `dump_observer` placed before `dump_thermo`. It also checks the last frame of each observer against a single-point evaluation of its model, for the energy, the potential energy in `observer.out`, the forces and the virial, and it checks the stress against the virial plus the kinetic tensor of the written velocities. The test fails against the `dump_observer` of master, and against versions that write into the thermo vector of the run, take the forces of the run for an observer, or take the virial of the run for an observer. `test_observer_of_temperature_nep` requires the observer energy of a temperature-dependent NEP to equal the energy in `thermo.out`. The force tolerance of 2e-4 eV/Å is shared with the existing observer test and sits above the measured residual of 2.1e-5 eV/Å.

`pytest -m "not slow"` in `tests_pytest` gives 662 passed, 478 skipped and 0 failed. `clang-format` leaves the changed C++ lines unchanged, and `flake8` reports nothing on the test file.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `eb7fbbf17a78`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.