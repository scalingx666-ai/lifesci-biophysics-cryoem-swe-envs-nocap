# Issue

**Title:** feat: SHINE trajectory clustering is implemented and example included

## Description

SHINE (Sampling Hierarchical Intrinsic *N*-ary Ensembles) is a class that performs hierarchical clustering on a set of pathways. It uses the *n*-ary similarity framework to sample/calculate the pairwise distances between the trajectories. The class also provides a method to generate a dendrogram plot of the clustering results.

Included in PR:
- BTS now has:
  - matrix refinement, which is critical for hierarchical clustering pairwise matrix generation
  - quota/diversity sampling (bin-wise method for sampling through data)
- SHINE clustering is implement in `src` under cluster module
- script is included to run shine under `scripts/shine`

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2f614b1caca7`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.