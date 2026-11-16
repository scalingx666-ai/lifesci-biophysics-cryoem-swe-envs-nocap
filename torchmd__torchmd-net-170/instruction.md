# Issue

**Title:** Fix double precision in Distance

## Description

Current Distance module fails when loop=True and positions have dtype=torch.double.
This PR fixes this by making sure edge_weight has the same dtype as edge_vectors in this specific case.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e02c076aefea`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.