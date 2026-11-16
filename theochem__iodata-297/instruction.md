# Issue

**Title:** Depend on importlib_resources for py37 for as_file api

## Description

This is a fix for py37 related to the recently merged #296

See https://github.com/theochem/iodata/actions/runs/4900272755/jobs/8750814221#step:6:933

I've locally reproduced the bug and tested the fix with py37.

It is a minor change, so I'll YOLO-merge this in the absence of comments after May 10.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `65408703d82b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.