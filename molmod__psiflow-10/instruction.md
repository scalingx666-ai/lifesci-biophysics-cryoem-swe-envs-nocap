# Issue

**Title:** Add missing type-hints to `apply_temperature_ramp`

## Description

I added the missing type-hints to `apply_temperature_ramp` since it was decorated by a typeguard. This will get rid of the warning message on importing `psiflow`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e011c2b17123`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.