# Issue

**Title:** Implement GH actions

## Description

Here's my take on GH actions for vermouth. Couple of things:

1) Appears you cannot have global environment/shell variables that are passed to the program calls. You need to feed them directly to the program called. But I don't know what calls the three variables listed atop. 

2) You cannot have allow failures in the same way as travis does it. There is skip upon error but that just tells the other tests to not stop when this particular test fails. In all other regards i.e. depending jobs will still be canceled and the test gets reported as failure. As such the PR will also become a failure. There is a open feature request for this.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b0f49b5aeac0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.