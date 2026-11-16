# Issue

**Title:** FIX: Redeclaration of the instance in Topology.bonds

## Description

The instances created by `bonds`, `angles` and `dihedrals` of `topology.topology.Topology` are always updated with each call.
So, I propose to redeclare a new instance at the time of the call.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2b24cb762865`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.