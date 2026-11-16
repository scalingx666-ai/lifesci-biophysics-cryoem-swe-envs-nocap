# Issue

**Title:** Switch analysis from u_kln to u_kn representation

## Description

Simpler than I thought, but this is distinct from the pymbar assumption since the `k` is per sampler, and the `l` is thermodynamic state

All methods except the _prepare_mar_input_data use the u_kln since they only take the input data, truncate/decorrelate, then return it.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8e46e99b58dc`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.