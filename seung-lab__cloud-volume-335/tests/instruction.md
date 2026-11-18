# Issue

**Title:** feat(microviewer): add segmentation toggle to plain segmentation

## Description

This implementation is a little slow right now and includes some duplicated code.

I realized we can avoid using hashes with a double array scheme.

1. renumber data
2. second array size of number of uniques contains original label

However, making this happen requires a slowdown in load time due to array renumbering. We should test a larger volume to see if this is a problem.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `646ec1e0459d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.