# Issue

**Title:** Unpin dependencies and cleanup

## Description

Most important change is that I unpinned two dependencies. I reinstalled schnetpack in a fresh environment with this change and did not experience any difficulty. Unfortunately it was not reported why these dependencies were pinned and it is possible that for some use-cases there may still be problems.

Other than that, I removed the `travis.yml` file, which is not needed anymore because CI on Travis is not done anymore, and `MANIFEST.in` which doesn't seem to be necessary since the switch to `pyproject.yaml`.

Also some whitespace removal and test readme update.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `05df07be19a0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.