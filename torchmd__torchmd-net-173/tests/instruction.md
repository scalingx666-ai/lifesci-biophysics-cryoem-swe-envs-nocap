# Issue

**Title:** Updating dependencies

## Description

This PR is an effort to take torchmd-net dependencies to the latest possible versions.
Mainly taking torch to 2.0.
After pressing the conda-forge guys to update some of the dependencies to torch 2 the env file I updated installs on my machine and all test pass. Lets see how the CI likes it :P

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `664a4d234a10`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.