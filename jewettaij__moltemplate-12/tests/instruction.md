# Issue

**Title:** MSI class2 force field converter and COMPASS force field

## Description

Initial version of the COMPASS force field ("compass_published.lt"), and the python script ("msifrc2lt.py") that generated it, as well as the original force field parameter file ("compass_published.frc").

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `00f1730cf6aa`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.