# Issue

**Title:** Fix for `.vis(..., type="morph")`

## Description

This bug occurs when an SWC has the same coordinate twice. This can happen if one loads with `read_swc(..., ncomp=2)` (or even larger numbers).

Thanks to Elisabeth Galyo for reporting.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5e9bd3914458`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.