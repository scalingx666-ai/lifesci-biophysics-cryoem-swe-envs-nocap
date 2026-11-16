# Issue

**Title:** fix ek friction forces

## Description

The ek lb friction force is now calculated by all ek species and not just from the last one, which previously had overwritten the forces of the other species

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `eea75a3aa810`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.