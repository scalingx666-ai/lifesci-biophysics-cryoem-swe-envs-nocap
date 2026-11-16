# Issue

**Title:** Fix per-type cutoffs with explicit charge_mode 0

## Description

### Summary

Explicitly setting `charge_mode 0` incorrectly rejects per-type cutoffs with "Can only use uniform cutoff for qNEP".
Omitting the keyword accepts the same input, although both configurations select plain NEP.

### Changes

Guard the qNEP reporting and cutoff restriction with `charge_mode > 0`.
This also suppresses irrelevant `lambda_q` and `lambda_z` output for plain NEP.
Modes 1 and 2 retain their cutoff restriction.
The explicit-setting flag remains available for model consistency checks.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `eb7fbbf17a78`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.