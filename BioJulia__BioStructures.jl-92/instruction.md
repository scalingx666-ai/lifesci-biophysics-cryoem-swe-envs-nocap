# Issue

**Title:** Detect and rename disulfide cysteines

## Description

Add disulfidebonds to pair cysteine SG atoms by distance and renamedisulfides! to apply Amber CYX residue names. Reject ambiguous pairings.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8802eb7083b0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.