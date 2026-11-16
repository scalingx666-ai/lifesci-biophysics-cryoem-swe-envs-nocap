# Issue

**Title:** Parsing PDB-IHM CIF files

## Description

Lenient cif parsing: allowing more missing fields. Also improved logging for the case of no atom_site

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b03fb3f57863`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.