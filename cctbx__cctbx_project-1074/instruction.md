# Issue

**Title:** Fixed out of memory error in cctbx.xfel.merge's isolation forest filter.

## Description

The isolation forest filter was having an out of memory error. The random forest models were very large. I set the number of estimators to 1,000 initially in development because it gave smooth decision boundaries. I reset this to 100 (sklearn's default). The problem was when the root rank communicated the models to the other ranks, the communication packet was just too large. I also changed from a comm.bcast to sending the model individually to the other ranks. I have found this to help with out of memory issues during mpi communications in the past.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1b6fc11f65fb`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.