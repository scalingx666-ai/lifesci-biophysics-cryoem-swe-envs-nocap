# Issue

**Title:** rdf action should be able to return the raw-rdf values

## Description

Hello everyone, 

I just updated the rdf action with an optional boolean argument `raw_rdf`.
When `raw_rdf` is set to True, the returned rdf values are not normalized, as the `rawrdf` argument in cpptraj.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `271eb49a2d16`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.