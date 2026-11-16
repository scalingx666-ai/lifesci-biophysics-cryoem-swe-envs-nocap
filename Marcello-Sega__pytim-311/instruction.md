# Issue

**Title:** fixed _read_frame_with_aux

## Description

.
The problem happened when patching the trajectory class, where the code tested only for the existence of the interface attribute (to avoid applying the patch twice), but was not replacing it with the new interface instance.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ea480bef7820`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.