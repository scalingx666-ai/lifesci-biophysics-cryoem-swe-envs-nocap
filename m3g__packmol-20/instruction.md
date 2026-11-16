# Issue

**Title:** CHARMM crd format enabled

## Description

I have performed all the required changes to create crd identical to CHARMM format.
The entries read in "record" are left aligned before writing crd format.

A new keyword segid is also added to be able to modify segment name written in third last column.
If no segid is specified, it remains same as resname read from file.
If segid is specified, it is kept the same for following "itypes" until another segid is encountered.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `add5deed4784`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.