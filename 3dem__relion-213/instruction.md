# Issue

**Title:** Calling relion_refine w/o args gives flags

## Description

The default call to usage() on ERROR report was disabled, but that
also means that the typical way of getting the flags was lost. This
fixes that by checking for

1. 	no flags
2. 	--help
3. 	-h

All the above three produces calls to usage()

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a2da0f6e92d6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.