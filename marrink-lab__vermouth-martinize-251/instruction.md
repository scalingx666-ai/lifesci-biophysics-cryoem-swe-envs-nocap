# Issue

**Title:** include all GROMACS types in the ffinput

## Description

@pckroon I have added all missing interaction types from the manual GROMACS 2020. I am not sure we will interpret all of them correctly but now the default behavior is the same as from before the refactoring. This will also fix #249 and the DNA building fail. 

Can you check that the dihedral interactions are located at the correct place? You are doing some stuff with the dihedral I do not fully understand something with the phase.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9c0022d1f57d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.