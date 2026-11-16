# Issue

**Title:** Update to use setup-miniconda v3; use miniforge

## Description

Mambaforge has been deprecated. See https://conda-forge.org/news/2024/07/29/sunsetting-mambaforge/

Now miniforge is effectively identical to the old mambaforge. At the same time, I'm updating the version action we're using.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2a1339e339c0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.