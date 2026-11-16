# Issue

**Title:** Removed lipid bilayer analysis helper

## Description

Regarding Issue #41, which was causing problems on sudo pip installs. Removed the load_scripts section from `gromacs/config.py` and `gromacs/tools.py` since there were no other scripts to load.

Followed list and description in Issue #41. A `sudo pip install` works now, but I have no tests to run to validate.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1f8cf0d1abd0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.