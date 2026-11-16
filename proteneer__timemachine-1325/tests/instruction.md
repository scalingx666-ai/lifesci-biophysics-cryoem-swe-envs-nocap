# Issue

**Title:** Hardcode n_frames_per_iter to 1

## Description

## Motivation

* Methods to extract trajectories by replica currently assume `n_frames_per_iter==1` and will break otherwise
* Since HREX now has minimal overhead, there's no clear use case for performing HREX moves less frequently than once per frame
* Hardcoding `n_frames_per_iter = 1` allows removing some subtle logic that exists to handle edge cases

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c30fffd18b07`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.