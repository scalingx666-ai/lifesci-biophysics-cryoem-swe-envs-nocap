# Issue

**Title:** Added compatibility with 4.x style tool referencing

## Description

Some scripts/canned commands rely on the 4.x program naming. `tools.G_rms` for example, which
is no longer the name of the rms gromacs command in 5.x. This creates an alias for all commands with the `G_` prefix. Like my last PR, I have no tests so am unsure if this will break any existing scripts.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `552a6966008f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.