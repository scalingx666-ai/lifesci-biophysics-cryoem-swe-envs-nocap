# Issue

**Title:** Use two-point numerical difference for debug gradients and commutative relative error calculation

## Description

This PR should solve the issue of running the gradient tests on ARM64 and possibly on x86-64 with the Intel compiler. The relative error is computed with a commutative formula from https://floating-point-gui.de/errors/comparison/. This PR also enables the tests for the orientation angle CVC.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `cd4bd64cbce7`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.