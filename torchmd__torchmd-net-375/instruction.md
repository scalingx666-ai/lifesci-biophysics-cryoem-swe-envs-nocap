# Issue

**Title:** Add Tensornet2 architecture

## Description

- adds the new tensornet2 architecture for the aceff-2.0 pre-trained model.

- the key addition is Aimnet2 style neutral charge equilibration and a coulomb interaction.

- This PR adds the tensonet2 model and a new output model that does a coulomb interaction using the predicted charges.

Note that the parent branch is #373

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `33447912a7b0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.