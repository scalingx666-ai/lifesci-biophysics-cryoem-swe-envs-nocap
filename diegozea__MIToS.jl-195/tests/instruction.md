# Issue

**Title:** Add support for annotated FASTA sequences

## Description

Added `AnnotatedFASTASequences` in `MIToS.MSA` to parse annotated sequence files in the Annotated FASTA Format (AFF) and the CAID modified FASTA reference format.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2cdce4454ceb`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.