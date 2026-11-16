# Issue

**Title:** Disable worker thread in PythonForce

## Description

This is a workaround for errors I'm getting (coordinates becoming nan) when using PythonForce to execute PyTorch code.  The errors only happen with the CUDA platform, not OpenCL.  Disabling the worker thread makes them go away.  I suspect they're in some way related to conflicts between OpenMM and PyTorch running CUDA code at the same time on different threads, but I haven't been able to figure out exactly what.  For the moment, I'm just disabling the worker thread.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c381ec78eb60`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.