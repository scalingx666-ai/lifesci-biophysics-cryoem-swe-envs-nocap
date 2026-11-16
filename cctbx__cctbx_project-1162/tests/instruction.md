# Issue

**Title:** Added the 'UHF' keyword for non-singlet calculations.

## Description

MOPAC defaults to RHF calculations, i.e. singlet state. If the multiplicitly is changed from singlet, this also requires the UHF keyword to be present.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `43170245ef6a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.