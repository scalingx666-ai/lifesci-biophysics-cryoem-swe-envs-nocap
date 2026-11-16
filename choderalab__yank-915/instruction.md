# Issue

**Title:** Unbias restraint analysis

## Description

Fix #882.

Not ready yet. I still have to add a bunch of tests (and fix the bugs), modify the CLI arguments, and do some cleanup, but this is a first draft of the implementation for the unbiasing of Harmonic/Flatbottom restraints. It also adds a system to invalidate automatically cached values once the attributes they depend on are changed. This allows, for example, to quickly compute free energy profile as a function of the restraint cutoff or the number of iterations, which could be performed only very very slowly before these changes.

Tests won't pass until choderalab/openmmtools#336 is merged, and a new openmmtools release is cut.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `30aacd0c56a1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.