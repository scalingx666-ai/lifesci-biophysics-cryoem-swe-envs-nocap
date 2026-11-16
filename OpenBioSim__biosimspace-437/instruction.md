# Issue

**Title:** Mirror functionality from Sire PR #348

## Description

This PR mirrors the functionality from OpenBioSIm/Sire#348, allowing the user to avoid writing velocities to the output files. This is useful when velocities overflow the fixed-width precision of the format, which would trigger an exception.

* I confirm that I have merged the latest version of `devel` into this branch before issuing this pull request (e.g. by running `git pull origin devel`): [y]
* I confirm that I have permission to release this code under the GPL3 license: [y]

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3653912d32a4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.