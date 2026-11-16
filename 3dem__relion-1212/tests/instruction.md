# Issue

**Title:** ctffind_runner better error message

## Description

Possible fix for issues like #1167? When CTFFIND fails to fit CTF parameters for all input micrographs (i.e. `MDctf` is empty) exit with an error rather than failing later.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `05c903d22025`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.