# Issue

**Title:** build: Update  build script to reflect new Colvars library location in the GROMACS worktree

## Description

See here:
https://gitlab.com/gromacs/gromacs/-/merge_requests/6316/diffs

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ad526c12b168`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.