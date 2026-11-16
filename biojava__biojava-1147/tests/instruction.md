# Issue

**Title:** Contact calculation performance tweaks

## Description

A few performance tweaks that increase contact calculation performance ~ 1.5x

- Use sqrt only when needed, use distance squared otherwise
- Primitive arrays instead of lists in GridCell
- Presize HashMap

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2a57e199c763`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.