# Issue

**Title:** Add newest CMake to container

## Description

GROMACS main now requres a newer CMake (which appears to track the latest Ubuntu version):

https://gitlab.com/gromacs/gromacs/-/commit/812d99f18f5aefd209c1364a0443276e2c360f28

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b5d8709e707b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.