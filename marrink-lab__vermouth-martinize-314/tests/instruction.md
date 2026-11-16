# Issue

**Title:** Limit the line length for the command in the top

## Description

Martinize2 writes the command it has been invoked with in
the header of the TOP file it writes. If that command is too
long, then it can pass the maximum line length that grompp
can read.

This commit truncate the command if it passes the limit.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c677535dca05`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.