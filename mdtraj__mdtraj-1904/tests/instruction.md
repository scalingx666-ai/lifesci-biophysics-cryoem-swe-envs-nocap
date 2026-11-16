# Issue

**Title:** fix pdbstructure "vmd convention" false-positive

## Description

So apparently the reader was misreading all residue numbers 2710+ as vmd overflow? Probably was too focused on getting it to work back in the day that I missed the potential false positives.

I was able to read/write the test files made by the same process in #1903. There was also a bug where the residue number for the TER line would overflowing if more that 9999 residues. This also fixes that.

```
# INCORRECT
TER   44980      HOH D14989
---
# CORRECT
TER   44980      HOH D4989

```

Updated some tests that will catch this as well.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ccd0c874361e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.