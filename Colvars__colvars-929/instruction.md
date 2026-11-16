# Issue

**Title:** Update Colvars_traj documentation

## Description

If I understand correctly, **Colvars_traj** creates a new instance of **Colvar_traj** for each collective variable. Right now the documentation reads like each collective variable is an instance of Colvars_traj. I think this is a typo, unless I'm not understanding the code (in which case, an explanation would be very much appreciated)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7f474d3b58f2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.