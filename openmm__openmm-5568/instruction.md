# Issue

**Title:** Fix CI

## Description

* A search suggests that `sudo apt-key del 7fa2af80` is a workaround for an issue installing CUDA that may no longer be necessary.  Apparently `apt-key` is deprecated as well and might have been removed from the runner.  This should fix Linux CUDA builds.
* Upgrades the conda installation actions; the docs build was using an old version that assumed conda would always already be present on the runner.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ad0c51cf0410`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.