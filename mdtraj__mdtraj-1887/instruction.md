# Issue

**Title:** fix editable mode, add packaging as dependency

## Description

#1850 

Added some try/except blocks during file copy/file name matching in `base_setup.py` as these steps are unnecessary (and fail) during editable mode install.

Also added 'packaging' as an install dependency because it's not default? (checked python 3.11 and 3.12), but needed in `mdtraj/formats/amberrst.py`.


To test, run `python -m pip install -e .`

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ae0214932e4f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.