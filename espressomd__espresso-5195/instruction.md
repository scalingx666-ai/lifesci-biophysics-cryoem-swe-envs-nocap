# Issue

**Title:** Fixed fene bond breakage on compression

## Description

Description of changes:
- Fixed bug in FENE bond force calculation that caused the bond not to break when the distance between particles is smaller than the bond compression threshold.
- Added new python integration test that tests if FENE bonds break as expected when elongated or compressed.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `dae3ef43790c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.