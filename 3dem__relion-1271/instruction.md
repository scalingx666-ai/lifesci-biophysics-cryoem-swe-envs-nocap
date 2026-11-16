# Issue

**Title:** Update eer_trajectory_handler.py (fix NumPy deprecation of np.int and np.float in RELION 5 conda)

## Description

Update eer_trajectory_handler.py to fix NumPy deprecation of np.int and np.float in RELION 5.0 conda environment.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3d6c20061d29`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.