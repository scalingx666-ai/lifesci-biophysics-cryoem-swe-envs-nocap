# Issue

**Title:** Rewrite of PDB status to use new RCSB PDB data API

## Description

This replaces the legacy RCSB PDB API by the current one, so that `PDBStatus` continues working after legacy APIs are shutdown in December.

This is a breaking change with a changed `Status` enum and 2 fewer methods (getReplace, getReplaces). All documented in CHANGELOG

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `65ad15fe58dc`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.