# Issue

**Title:** add verbosity argument to gen_traj_numpy()

## Description

gen_traj_numpy() was clogging up my stdout when I was using it a lot so I added an option to skip printing information about the system. I set the default verbosity argument value to True so that the default behavior of the function wouldn't change.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `fe7485e32104`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.