# Issue

**Title:** Correct ENSEMBLE central-moment and normalized-weight derivatives

## Description

## Summary

Correct the derivatives of ENSEMBLE central moments and reweighted observables:

- Include the derivative of the replica-averaged mean in central moments.
- Differentiate normalized reweighting weights for both raw and central moments.
- Apply the POWER chain rule to derivatives with respect to the bias-energy argument.

Observable values and syntax are unchanged. Derivatives, and therefore forces when these observables are biased, can change.

## Target and tests

This targets v2.9, the oldest maintained affected series, following CONTRIBUTING.md. The fix was also built and tested on current master.

The new MPI regression uses finite differences of the resulting bias energy with respect to both the observable and reweighting-energy inputs. It covers weighted/unweighted ensembles, central/raw moments of order 2 and 3, and powers 1 and 2.

- Native v2.9 C++14 MPI build passed.
- `make -C regtest/basic/rt-ensemble-derivatives test` passed with 2 replicas x 2 ranks.
- The same regression passed with 3 replicas x 2 ranks.
- Existing `basic/rt65-mpi` and `basic/rt74` regressions passed.
- Bundled astyle reported both changed C++ files unchanged.

No regression tolerance was relaxed. This PR is independent of other proposed PLUMED changes.

## CI follow-up

The independently reviewable test-initialization fix #1464 is now included in this branch. It initializes the existing timestep regression cell/virial; the deterministic poisoned-stack negative/positive controls show the original test fails and the corrected test matches the original references. No production behavior or tolerance is changed by that companion commit. GNU Autoconf download timeouts in `codecheck` occur before source checks and remain an external CI limitation.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d650dbd5a52e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.