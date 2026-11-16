# Issue

**Title:** Update Hydrophobic and HBond SMARTS

## Description

-  and #214 
- Fixes Hydrophobic SMARTS pattern relying on the `D` token: due to the presence of explicit hydrogen atoms this doesn't work as expected.
- Excludes triazolium, and guanidine/anidine-like from HBond acceptors (also explicitly excluded **charged** aromatic nitrogen, was previously implicit from `!$([nX3])`)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `dcaafdc9b6d3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.