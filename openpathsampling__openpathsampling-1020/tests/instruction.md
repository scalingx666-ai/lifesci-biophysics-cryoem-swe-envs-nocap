# Issue

**Title:** Fix error in testing OpenMM RCs

## Description

I used the wrong trigger name in #1019. (I had also looked at using the `repository_dispatch` mechanism, but decided to use `workflow_dispatch` instead.)

I'm merge this as soon as tests pass.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `804683f99fcf`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.