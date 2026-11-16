# Issue

**Title:** solid: estimate spring constants from the unrounded MSD

## Description

## Problem

`Solid.analyse_spring_constants` rounded the mean MSD to 2 decimals. Below 0.005 Å² (low temperature, heavy or stiff elements) the MSD rounded to 0. The code then substituted 1 Å², giving k = 3kT / 1 Å². With an MSD of 0.004 Å² at 100 K, that makes k 0.026 eV/Å² instead of 6.46 eV/Å². `validate_spring_constants` accepted the value because it lies inside [1e-4, 1e3], so the run went on with a badly matched Einstein crystal, large dissipation and a biased free energy. Only a warning was logged.

Two related problems:
- The convergence check compared the rounded MSD standard deviation against `tolerance.spring_constant`. That standard deviation is almost always rounded to 0, so the loop stopped on the first cycle.
- If the loop ran out of `n_cycles`, `self.k` was never set and the run crashed later.

Between 0.005 and 0.05 Å², the rounding made k coarse (up to about ±50 %) but did not bias the free energy.

## Fix

- Use the unrounded MSD. An MSD that is zero or not finite gives k = nan for that element. `validate_spring_constants` replaces it with another element's k (the case of an element without atoms, which the old fallback was there for), or raises when no element has a usable value.
- Converge on the relative change of k between cycles, compared against `tolerance.spring_constant` (default 0.1). k only needs to be approximate, so this normally stops at cycle 2.
- The loop is still bounded by `md.n_cycles`. If that runs out, the last estimate is used and a warning is logged.

Workaround for affected versions: set `spring_constants` explicitly.

## Behaviour change

- With default settings, the spring constant loop now runs two cycles instead of one, so one extra `n_small_steps` run.
- Spring constants differ slightly from earlier versions because the MSD is no longer rounded.
- Single-element runs with a zero MSD now stop with an error instead of running with k = 3kT / 1 Å².

## Tests

- `tests/test_spring_constants.py`: 7 tests covering a small MSD, no rounding, zero MSD for one or all elements, user-supplied k overriding the estimate, stopping after two cycles, and the `n_cycles` cap. All 7 fail on `main`.
- Golden command streams are unchanged.
- Full suite including the LAMMPS lane: 474 passed, 3 skipped.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `67caa5becc35`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.