# Issue

**Title:** make zenodo archive title identical to JOSS paper title

## Description

- close #402
- use same title in CITATION.cff for paper and project (required for the JOSS paper publication #71)
- added DOI for preferred citation in note because that is the only hint that shows up on zenodo)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `882ecf9d3a65`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.