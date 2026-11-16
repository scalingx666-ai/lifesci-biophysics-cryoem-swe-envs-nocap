# Issue

**Title:** allows zero surface atoms in gitim

## Description

A bulk simulation without voids larger than the probe sphere radius
does not have any surface

* previous behavior: MDAnalysis raised a ValueError
* new behavior: return empty surface groups

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7c98a7047b41`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.