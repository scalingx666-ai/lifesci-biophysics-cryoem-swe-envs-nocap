# Issue

**Title:** Fix docstring MDTopology to MDTrajTopology

## Description

A student asked my help initializing the `OpenMMEngine` and I could not find the `openpathsampling.engines.openmm.MDTopology`. Looking at `openpathsampling/engines/openmm/topology.py` this is actually called: `MDTrajTopology`

This PR all the docstrings that mention `MDTopology`. 
it also fixes some of the trivial `pep8` complaints in openmm/engine.py

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `55716a0b9c2f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.