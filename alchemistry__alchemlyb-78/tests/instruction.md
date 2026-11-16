# Issue

**Title:** Add gomc parser

## Description

Add GOMC parser to alchemlyb. Energy output file for Free Energy Calculation is similar to GROMCAS. This parser, reads, Total energy, PV, derivative of energy (for current lambda state), and change of energy (between current lambda state and all other lambda states).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `86d2d26d4407`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.