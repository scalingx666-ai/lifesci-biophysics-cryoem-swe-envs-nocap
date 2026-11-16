# Issue

**Title:** Recognize specialized protein residue names

## Description

Include terminal and disulfide-bonded residue names in `proteinresnames`, and map terminal variants to their base residue for amino acid property selectors.

Fix terminal specialization for space-padded PDB atom names and re-read the residue name after specializing histidine protonation so terminal histidines can be specialized correctly.

Assisted-by: Claude Opus 5 <noreply@anthropic.com>

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e3f14d0e8260`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.