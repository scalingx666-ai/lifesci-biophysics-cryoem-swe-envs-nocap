# Issue

**Title:** Enhancement/opt convergence

## Description

Enhancement: convergence of DFT optimization in the molecules having a linear angle.
Enhancement: changing behavior of psi4_mp=1 in conformation_search to avoid out of memory during psi4 running.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e4b94e23830f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.