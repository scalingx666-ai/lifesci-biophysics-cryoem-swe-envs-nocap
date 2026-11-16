# Issue

**Title:** Change naming of Cterm oxygens to make DSSP happy

## Description

DSSP requires at least one O atom in every residue, even for Cterminal atoms. So rename O1/O2 to O/OXT.
This probably caused quite a lot of issues for many people. DSSP 3 seems to trash just that residue, DSSP 2 might not work at all (molecule is not a protein). That means this is a fairly high priority hotfix.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c744470ac8fe`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.