# Issue

**Title:** elastic_constants: relaxations converge to a part of the forces the strain makes

## Description

A bug fix found by the elastic-constants study.

**The bug.** `elastic_constants` relaxed the atoms under each strain until the largest force fell below `max_force` (1e-4 eV/Å by default). The forces a small strain makes are themselves small, about 1.8e-2 eV/Å for silicon at the default strain of 1e-3, so the relaxations stopped with the atoms half relaxed. The effect is largest where the atoms relax most. C44 of silicon with the Tersoff potential (92 GPa unrelaxed, 10.31 relaxed) came out:

| strain | max_force 1e-4 (before) | 1e-6 | 1e-8 |
|---|---|---|---|
| 1e-2 | 10.275 | 10.318 | 10.318 |
| 1e-3 (default) | 10.205 | 10.312 | 10.310 |
| 1e-4 | 13.933 | 10.268 | 10.310 |

**The fix.** Each relaxation now converges until the forces left are 1e-4 of the largest force the strain makes, if that is below `max_force`. It never goes below 1e-3 `max_force`, so it does not chase rounding where a strain moves no atoms, as in monatomic cubic crystals. A crystal whose atoms do not relax stops at once, as before.

**Test.** The new test in `test_elastic_lammps.py` uses Tersoff silicon in LAMMPS. C44 at strains of 1e-3 and 1e-4 with the defaults equals C44 from tight relaxations within 1e-3; before, the two strains gave 10.20 and 13.9. The lattice-dynamics, docs and API suites pass locally (480 tests).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a8a9282e4b1a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.