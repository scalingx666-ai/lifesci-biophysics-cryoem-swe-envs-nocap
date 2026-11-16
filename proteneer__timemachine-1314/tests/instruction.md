# Issue

**Title:** Adds normalized kl divergence metric

## Description

Had previously looked at the KL Divergence for detecting bottlenecks in HREX and it seemed to grow unbounded with the number of windows, which is a problem when wanting to compare edges with different number of windows. Normalizing it appears to do the right thing

## Unnormalized
![image](https://github.com/proteneer/timemachine/assets/5840832/4e074224-f80b-4044-adf6-76c6c31df31d)

## Normalized
![image](https://github.com/proteneer/timemachine/assets/5840832/6e9d905c-a437-41a7-a4d6-e9dffe29079f)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d211aafd3470`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.