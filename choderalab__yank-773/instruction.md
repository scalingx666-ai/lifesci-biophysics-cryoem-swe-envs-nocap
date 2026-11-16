# Issue

**Title:** Various fixes for trailblaze scheme and loading local leap parameter dat file

## Description

Requires a new `openmmtools` release for tests to pass.

- Fix loading of leap parameters from a local `.dat` files (allow us to use local versions of gaff parameters for validation).
- Fix #772: Use infinity instead of `None` to specify unlimited number of iterations. Should fix also the crash described in #771 (testing this now).
- Fix #762: Trailblaze protocol crashes with MPI.
- Generalized restrained atoms selection during trailblaze scheme to include non-protein receptors (see also choderalab/openmmtools#290).
- Fixed bug when computing reduced potentials of simulated energies. This should solve the problem described in #766.
- Fix #763: Automatic path is saved in YAML as a mix of python and numpy floats.

This may solve also the problem in #769 even though I couldn't reproduce it so I'm not certain.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `85dcf5fabcbf`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.