# Issue

**Title:** address issue with windows basepath extraction

## Description

To address: , an issue where windows paths were not parsed properly. Specifically, the bucket was getting added to the cloudpath twice.   

I added a simple test to make sure the fix works.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a371d34d7c04`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.