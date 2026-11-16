# Issue

**Title:** Parsing resolution values in PDB (REMARK 2 and REMARK 3 [new format]) and mmCif

## Description

Apparently it affects only cryo-EM entries.

Partially  
It fixes the PDBFileParser.
Before fixing the mmCifParser we need first to check whether the files should be remediated or not.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6c5761c28468`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.