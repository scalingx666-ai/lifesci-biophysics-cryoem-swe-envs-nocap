# Issue

**Title:** Remove ff19ipq

## Description

ff19ipq turns out to be an incomplete and abandoned force field: see the discussion at openmm/openmmforcefields#410.  It is possible it was added to the AmberTools distribution by mistake; we added it to OpenMM after adding it to openmmforcefields.  This removes the force field file.  See also openmm/openmmforcefields#419.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `98173c604129`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.