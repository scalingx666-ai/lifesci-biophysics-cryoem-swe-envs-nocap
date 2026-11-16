# Issue

**Title:** Update md17

## Description

The file names used in the MD17 dataloader (torchmdnet/datasets/md17.py) have become obsolete. This pull request (PR) aims to update all the file names in accordance with the latest release available at http://www.quantum-machine.org/gdml/data/npz/.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `664a4d234a10`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.