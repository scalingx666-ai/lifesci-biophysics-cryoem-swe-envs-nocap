# Issue

**Title:** Sort nodes to group residues

## Description

Adds a processor to assign consecutive atomids to molecules, sorted by chain and resid. That way residues will be grouped together in the resulting PDB and ITP file(s).

@fgrunewald Is this more or less as you intended? If yes I'll add some tests.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `19c8aa74e745`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.