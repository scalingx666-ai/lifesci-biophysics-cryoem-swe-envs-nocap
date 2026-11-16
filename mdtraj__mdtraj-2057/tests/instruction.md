# Issue

**Title:** Add support for "standard_names" keyword argument to Amber prmtop reader

## Description

Addressing issue #2056 Amber prmtop reader now supports the "standard_names" keyword argument in the same way as the PDB reader does, so users have a choice over whether non-standard protein atom and residues names in Amber .prmtop files are translated into their standard equivalents, or not.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `99f60b649ae1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.