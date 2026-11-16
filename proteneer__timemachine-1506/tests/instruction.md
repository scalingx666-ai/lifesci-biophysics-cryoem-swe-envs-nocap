# Issue

**Title:** Verify Bitwise Reproducibility of RBFE Endstate Trajectories

## Description

* Adds a `pytest-archive` directory to CI to get out artifacts, in case we can't emulate the CI (which I can't seem to do for RBFE, while I can for the Buckyball test case)
* Adds hashes of the endstates of RBFE runs so we can identify when simulations changes. Decided on hashes since the file sizes are large. This does **not** detect changes to the dGs.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `94ee21909118`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.