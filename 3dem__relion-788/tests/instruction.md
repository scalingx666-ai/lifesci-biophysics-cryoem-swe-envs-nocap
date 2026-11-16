# Issue

**Title:** Fix for race condition during cleanup of local scratch.

## Description

After a classification/refinement using a scratch directory is finished, each worker rank tries to delete the scratch directory.
This can lead to a race condition, where some of the rm -fr commands fail and where Relion aborts and marks the job as failed.
Setting keep_scratch=true for all ranks except the first on each node avoids the race condition, as only one rank per node performs the cleanup.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0e9e832687d6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.