# Issue

**Title:** Add option to impose RMSD restraint on core heavy atoms and nearby protein CA atoms

## Description

This PR adds the option for imposing an RMSD restraint on core heavy atoms and nearby protein CA atoms.

Enabling this option also adds a virtual (zero force) bond between the small molecule and protein, and between any protein chains, ensuring the whole system is imaged together.

@dominicrufa : Any suggestions on where to add a unit test?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `29abd1a7b87a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.