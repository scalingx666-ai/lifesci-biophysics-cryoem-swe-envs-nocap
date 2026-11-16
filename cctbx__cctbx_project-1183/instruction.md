# Issue

**Title:** Don't add phantom hydrogens to water oxygens in reduce2 when there ar…

## Description

…e already bonded atoms to the waters, so we don't duplicate hydrogens.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8ea4908975e7`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.