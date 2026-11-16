# Issue

**Title:** Connect options

## Description

Add `ignore_conect` and `non_standard_conect` options. The first
will just make packmol ignore all CONECT lines. The second will use a non-standard reading of the 
lines, in which the atom indices are expected to be separated by spaces. The default is `ignore_conect = .false.`
and `non_standard_conect = .false.`, such that the CONECT lines are not ignored and the standard PDB
format is expected.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `72a9866662ca`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.