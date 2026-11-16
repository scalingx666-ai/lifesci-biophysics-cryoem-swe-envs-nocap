# Issue

**Title:** Do not use development-facing `openff-units`

## Description

- [x] Do not separately test against an older `openff-units` and a development branch [^1]
- [x] Work around known CI-facing bug 


[^1]: If we do want to test against both versions, there's a cleaner way to do that

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `84bc8757e942`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.