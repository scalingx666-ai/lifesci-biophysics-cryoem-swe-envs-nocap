# Issue

**Title:** updated RTD config

## Description

- close #257 
- move requirement files to ci/readthedocs
- renamed readthedocs.yml to .readthedocs.yml
- removed deprecated system_packages from config and explicitly added packages to the requirements.txt file
- pinned numkit everywhere to >= 1.0 for consistency

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `782303571592`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.