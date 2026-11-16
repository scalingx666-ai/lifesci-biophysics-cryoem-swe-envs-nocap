# Issue

**Title:** Reduce2 skip bad atoms

## Description

Has reduce2 continuing to run with default values for atoms whose hydrogen-bonding status or other characteristics cannot be determined (unknown HET groups). This enables reduce2 to run on a wider range of models with the goal of running on all models in the PDB and corner cases.

Prints all of the warnings on the console and the description output with Warning: prefixed to indicate the problems to the person running it.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c70152f45602`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.