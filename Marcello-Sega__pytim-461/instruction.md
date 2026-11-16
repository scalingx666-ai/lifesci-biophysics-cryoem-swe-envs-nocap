# Issue

**Title:** integrating pytim-notebooks

## Description

This revision adds pytim-notebooks to the CI workflow.
The repository https://github.com/Marcello-Sega/pytim-notebooks is cloned and the notebooks are executed

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `89a54573a7f4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.