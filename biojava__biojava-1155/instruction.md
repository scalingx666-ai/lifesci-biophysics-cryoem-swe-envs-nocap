# Issue

**Title:** More performant interface clustering

## Description

Using a leader clustering algorithm. Works well in all cases I know for the task of interface clustering, be it for all interfaces of an assembly or all interfaces found in a crystal. The performance difference from the previous version is stark for cases with many interfaces, see TestInterfaceClusteringPerformance

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a9dd08f7bbd8`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.