# Issue

**Title:** Repair CI

## Description

Restore the repository's supported CI build configurations.

- CI jobs selected earlier action and runtime releases. Update workflow actions and runtime images to supported releases. : ,
- CI configured build variants without their required native dependencies. Install only the dependencies required by each build variant. : ,
- CPU-only jobs left optional setup enabled and did not select their Python interpreter. Disable unused setup and provide the system Python paths. : ,

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f6d3ad20cde0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.