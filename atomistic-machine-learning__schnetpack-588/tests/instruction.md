# Issue

**Title:** requirements.txt is linked to setup.py

## Description

what do you think of doing it this way.
pip install -r requirements.txt will basically run setup.py now

alternatively you could parse the requirements.txt file in setup.py but that is not recommended

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d91ec6a65a9a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.