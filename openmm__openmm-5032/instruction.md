# Issue

**Title:** fix for scaled 1-4 LJ interactions when NBFIX is present in Gromacs topology file

## Description

This fix correctly scales LJ 1-4 interaction when reading a Gromacs topology file where NBFIX is present, pair parameters are not given via pairtype, and scaling of LJ 1-4 interactions is not 1.0. This is needed to correctly read Amber-type force fields that use NBFIX, for example a99sbws-tip4p2005s.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0a4edb841bb5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.