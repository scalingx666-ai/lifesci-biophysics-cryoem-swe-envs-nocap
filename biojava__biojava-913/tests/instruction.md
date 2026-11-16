# Issue

**Title:** Performance improvement in ASA calculation

## Description

Implemented some of the ideas exposed in Eisenhaber et al 1994 (https://onlinelibrary.wiley.com/doi/epdf/10.1002/jcc.540160303). This results in ~x5 runtime improvement

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c30ab8f4a329`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.