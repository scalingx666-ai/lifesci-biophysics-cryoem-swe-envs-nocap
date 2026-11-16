# Issue

**Title:** Residue templates can specify constraints

## Description

This is another change to support future OpenFF force fields.  A template can now include `<Constraint>` tags which specify constraints explicitly required by the force field.

cc @j-wags @IAlibay

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8eeee16de9bc`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.