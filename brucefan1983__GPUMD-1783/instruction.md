# Issue

**Title:** Loosen the TNEP tolerances to the cross-build noise floor

## Description

## Summary

The two TNEP test modules compare against numbers produced by one particular build. The golden
comparison in `test_io_tnep_commands.py` holds `dipole.out` and `polarizability.out` against a
reference file in `fixtures/golden/`, and the rotation check in `test_tnep_invariances.py` holds
a rotated run against the unrotated one. Both bounds were set from the run-to-run spread of a
single binary, which is zero: five repeats of each case on one GPU reproduce the dumped values
bit for bit. The spread a test has to absorb is instead the one between binaries, since another
CUDA toolkit, another GPU architecture or a change in the evaluation code reorders the fp32
arithmetic, and the five velocity-Verlet steps ahead of the last dumped row amplify it.

This MR raises both tolerances to an order of magnitude above the differences measured against
the current binary. `TNEP_GOLDEN_TOLERANCE` is raised from `rtol=1e-6, atol=1e-8` to
`rtol=1e-4, atol=1e-5`, and the `atol` of `TNEP_ROTATION_TOLERANCE` from `1e-5` to `1e-4`. In
both cases `atol` is what carries the near-zero components, where a relative bound alone is
unstable: the off-diagonal components of the polarizability are of order 1e-3 to 1e-2 against a
diagonal of order 1e2. The comments beside the two constants record the measured differences
they are set from. The style fixes in the same two files are unrelated to the tolerances and
touch a docstring line, a comment and one continuation indent.

## Testing

Measured against the golden file, the difference of the current binary reaches 1.1e-6 for the
dipole, whose components are of order 1, and 4.6e-6 for the polarizability, whose off-diagonal
components stay within 7.7e-7. The rotation check passed before this MR with a margin of 1.85
over its bound, which is too thin to survive a rebuild elsewhere, and now has a margin of about
10. Perturbing the golden values by a relative 1e-3 still fails both cases, so the tests keep
their bite at that scale; a relative 1e-4 passes, which is the sensitivity `rtol=1e-4` sets.

Gates run on an RTX 5080 with CUDA 13.3:

- `pytest test_io_tnep_commands.py test_tnep_invariances.py`: 4 passed.
- `pytest -m "not slow"` over `tests_pytest/`: 680 passed, 349 skipped, 9 deselected, in 411 s.
  The skips are the tests that need `netCDF4` and `MDAnalysis`, which this machine lacks.
- `flake8 --max-line-length=100` on the two files: clean.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e5a0e3595199`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.