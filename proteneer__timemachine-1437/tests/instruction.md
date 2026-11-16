# Issue

**Title:** Pip install OpenMM

## Description

* Upgrades OpenMM from 8.0.0 to 8.2.0
* As of [8.2.0](https://github.com/openmm/openmm/releases/tag/8.2.0), can pip install OpenMM. Avoids the need to do a custom build and doesn't install its own version of Cuda.
* Reduces docker container size by 0.1GB by avoiding the custom OpenMM build

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `92e25e02ec00`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.