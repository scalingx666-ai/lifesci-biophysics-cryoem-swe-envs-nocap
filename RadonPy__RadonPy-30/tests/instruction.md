# Issue

**Title:** Release/0.2.6

## Description

Enhancement: convergence of DFT optimization in the molecules having a linear angle.
Enhancement: changing behavior of psi4_mp=1 in conformation_search to avoid out of memory during psi4 running.
Enhancement: changing behavior of mp=1 in polarizability calculation  to avoid out of memory during psi4 running.
Bug fix: for sample scripts

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `fbf75adc8ae1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.