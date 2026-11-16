# Issue

**Title:** Fix Fah generator to only generate yaml-specified directories

## Description

- Only generate necessary directories (complex, solvent, apo, vacuum) based on yaml file
- ~Allow removal of HBond constraints~

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `85aa6af259db`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.