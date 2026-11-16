# Issue

**Title:** Add HIS(charmm)->HSE(martini22) mapping

## Description

So somewhere between v0.7.3 and v0.8.1 the martini22 forcefield lost its HIS block (it got renamed to HSE). Because of this there no longer was a mapping charmm(HIS)->martini22(HIS). The fix here is to add the mapping HIS->martini22(HSE) from the atomistic force fields.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `85c867caaa78`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.