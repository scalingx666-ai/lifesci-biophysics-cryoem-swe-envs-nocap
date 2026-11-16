# Issue

**Title:** Optimization of subunit clusterer for quaternary sym detection of PDB deposited structures

## Description

This pull request introduces a new switch for the subunit clusterer used for symmetry detection. If the switch `useEntityIdForSeqIdentityDetermination` is enabled, it uses the entity id of the subunits to establish identity of sequences, saving the full Smith-Waterman alignment calculation.

This optimization is important in cases like large viral capsids, where there are many thousands of chains and all-to-all sequence alignments become a bottleneck. E.g. for 6Q1F the runtime goes from ~ 6 hours to 7 minutes.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4c7c62e4d29d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.