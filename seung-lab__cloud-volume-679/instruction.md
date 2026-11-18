# Issue

**Title:** fixed existing graphene meshes reported as missing

## Description

Using `mesh.exists` with a graphene cloudvolume would return `None` for existing meshes. 
The if statement in the `remainder_labels` check caused only labels that were already found with `dynamic_exists` to be passed to `initial_exists`. This caused meshes to be reported as missing if they were missing from the initial but not dynamic meshes, or vice versa.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9b53280a25ca`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.