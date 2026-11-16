# Issue

**Title:** Itp reader issues

## Description

This fixes a bunch of issues related to the itp parser:

- parse '#else' directives as mentioned in #362
- increase speed of parser by fixing #377 
- parse directives position_restraints, settles, dihedral_restraints
- increase speed of make_residue graph #379 

Open issues are:
- parsing cmap #257 
- nested ifdefs fail #284

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `cf7dac3dd149`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.