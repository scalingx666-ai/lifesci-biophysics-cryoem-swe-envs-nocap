# Issue

**Title:** Augment --jobid to take the contents of an environment variable if desired

## Description

This adds the ability to specify `--jobid=ENVNAME`, where `ENVNAME` is the name of a zero-indexed environment variable.

How do we test `--jobid` in the units tests? I was looking to add a unit test, but couldn't find where the `--jobid` tests were.

This feature is needed for ORNL TITAN.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f3ff02f293bb`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.