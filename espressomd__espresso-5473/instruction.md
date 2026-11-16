# Issue

**Title:** fix lb thermostat for tau > time_step

## Description

The lb thermostat did not work correctly when tau and timestep did not match, as only every Nth timestep the particle obtain the lb forces. The fix here is to rescale the thermal coupling force by the a factor between tau and time_step.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `39a185f73bc9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.