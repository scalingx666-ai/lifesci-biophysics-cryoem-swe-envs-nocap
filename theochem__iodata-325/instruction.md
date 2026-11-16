# Issue

**Title:** Update installation instructions

## Description

Another step in #313 (before the next prerelease. I'm aiming for Wednesday, June 12)

These instructions are intended to work with the next prerelease:
- I'm not preparing conda releases at this stage. (Not so useful anymore now that IOData is pure Python. Pip works in a conda env.)
- I've updated NumPy and SciPy dependencies to the oldest ones that are not listed on the NIST national vulnerability database. It may be worth extending our CI to explicitly test with these verions.
- The `--pre` option is no longer needed.
- The number installation options has been reduced:
  - No conda releases.
  - We no longer have setup.py (because such setup scripts are no longer recommended).
  - The option `--user` is no longer needed. `pip` assumes it when there is no venv.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2ba38581ef3e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.