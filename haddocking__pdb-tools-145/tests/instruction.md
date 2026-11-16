# Issue

**Title:** Use HEADER info if available to name FASTA records.

## Description

.

If `HEADER` is available and it contains a non-empty PDB ID record (cols 63-66), then use this as a prefix for the FASTA records. Otherwise, default to 'PDB'.

Chose not to implement as an option since `pdb_tofasta` already has an option (`-multi`) and this is actually what makes the most sense.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b4b7010995a2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.