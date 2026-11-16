# Issue

**Title:** [WIP] Enable hybrid topology factory to be repartitioned at the endstates

## Description

@dominicrufa and I have added a class to `relative.py` called `RepartitionedHybridTopologyFactory` which enables the endstates to be repartitioned. 

This will be useful for incorporating BLUES and/or REST into Perses, which will enhance sidechain rotamer sampling at the endstates, a necessary feature for protein mutation free energy calculations.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `317a3bbfdc79`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.