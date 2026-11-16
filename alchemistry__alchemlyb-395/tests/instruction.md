# Issue

**Title:** add CITATION.cff file

## Description

- close #394 
- add all AUTHORS with ORCIDs and affiliation (as far as they are confirmed)
  - order based on AUTHORS
- add alchemlyb PIs
- add contributors that were listed in JOSS paper acknowledgements (PR #328) but had NOT been listed in AUTHORS (inserted in chronological order)
- only add emails for maintainers/PIs
- add Software Heritage Identifier
- add paper citation section for MBAR, decorrelation, and JOSS paper #328 

-----

- add one new contributor (from recent PR #381 ) to AUTHORS and CITATIONS.cff

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b1b6d4f58a7c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.