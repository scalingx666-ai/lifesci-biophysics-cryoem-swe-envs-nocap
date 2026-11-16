# Issue

**Title:** fixed find_gromacs_command()

## Description

- run.find_gromacs_command() was still using the old gmx:mdrun driver/command syntax
-

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1328b4170674`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.