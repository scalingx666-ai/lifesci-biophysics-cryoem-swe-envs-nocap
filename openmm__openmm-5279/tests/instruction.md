# Issue

**Title:** Make sure contexts are deselected before evaluation

## Description

.  Ensures that the (CUDA) context is deselected before `LocalEnergyMinimizer` calls `calcForcesAndEnergy`.  There are several other places in the code where this occurs (and which can cause a `PythonForce` callback to be invoked with OpenMM's CUDA context on the stack), so I created a `ContextDeselector` to pop a context and push it again.  If there is a better way to accomplish this, I could use it instead of adding this class, but trying to use only `ContextSelector` made some methods very messy.

The checks in `PythonForce` and `CustomCPPForce` for other instances of these forces should also now not be triggered when they find themselves.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0aee805025cc`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.