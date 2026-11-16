# Issue

**Title:** Removed dynamic typing mechanism in CompoundState to fix pickling.

## Description

Should be ready for review as soon as tests pass.

I had to convert the `ThermodynamicState._standardize_system()` method from class to instance method to make the change. All state classes are now tested for pickling.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `64fdc10ad3b0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.