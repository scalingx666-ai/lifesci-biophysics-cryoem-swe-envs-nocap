# Issue

**Title:** Fix force invalidation in CustomVolumeForce

## Description

After #5440, CustomVolumeForceImpl derives directly from ForceImpl and inherits updateContextState(), which sets forcesInvalid = true. That unnecessarily invalidates cached forces. Commit restores the previous no-op behavior and adds a regression test.  

(Found by my automation AI-assisted checkout code review)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0c4bcaba734e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.