# Issue

**Title:** Update dependency requirements

## Description

Prepare ESPResSo for a dev-EESSI release.

Description of changes:
- bump HighFive version requirements
- allow Cython 3.1.x and Cython 3.0.4 for foss/2023b
- make Gay-Berne potential part of the default myconfig file
- fix CMake parsing of environment variable `ESPRESSO_MYCONFIG`
- limit testsuite to 3 cores on the macOS runner to avoid timeouts

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3f929c402ea9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.