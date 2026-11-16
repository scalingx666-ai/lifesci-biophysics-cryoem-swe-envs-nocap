# Issue

**Title:** Replace `xdrlib` with `mda_xdrlib`

## Description

I'm trying to reduce some of the deprecation warnings we get in the mosdef packages, and one of them was from mdtraj's use of `xdrlib`.   mentions using the `mda_xdrlib` package as a replacement, which this PR does.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `934b2732aa7a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.