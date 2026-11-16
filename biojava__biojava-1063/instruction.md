# Issue

**Title:** Some more categories written out to CIF

## Description

Adding `_entity` and `_entity_poly` to CIF output, so that some more metadata is available for consumers.

Also: 

- now adding seqres groups to `Chain`s when no entity info is present in input file
- upgrading to latest ciftools-java

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `88b51deb9375`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.