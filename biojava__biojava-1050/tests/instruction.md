# Issue

**Title:** Bump dependency ciftools-java-jdk8 to 4.0.5

## Description

Bump dependency 4.0.5 to Fix cif writing in non-US locales.

Thanks @JonStargaryen for fixing the bug at **ciftools-java**.
.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4eac468cae5a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.