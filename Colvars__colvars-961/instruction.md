# Issue

**Title:** Check that the restarted step value is consistent with MTS settings

## Description

This PR addresses the potential case when the "absolute" step number (which continues from the Colvars state file, the NAMD `firstTimeStep` or LAMMPS checkpoint) is not divisible by the MTS frequency.

Additionally, add the note that was previously missing that `globalMasterFrequency` in NAMD sets a hard lower limit on `timeStepFactor`

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b2e27699a21a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.