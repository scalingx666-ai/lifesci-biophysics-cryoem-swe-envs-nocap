# Issue

**Title:** Fix dataclass member names for masks. Mildly concerning these aren't caught by static type checking...?

## Description

Fix dataclass member names for masks. Mildly concerning these aren't caught by static type checking...?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `839340e65167`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.