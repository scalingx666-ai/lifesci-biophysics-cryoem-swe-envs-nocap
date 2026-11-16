# Issue

**Title:** Cluster selection for GITIM/SASA

## Description

Allows to perform interface determination
on a selection of clusters, e.g.,
the set of n_clusters largest clusters:

 n_clusters = 2

or by passing the minimum number of atoms in the
clusters to be considered:

 min_cluster_size=300

(Test added)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d0fabf58dfa6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.