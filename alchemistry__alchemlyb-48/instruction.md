# Issue

**Title:** Issue 14 expanded ensemble parser

## Description

Modified gmx parser to handle expanded ensemble simulation data. Original parser relies on the state being disclosed in the subtitle line of the dhdl.xvg, which is not the case in expanded ensemble datasets. Instead, the states are referenced in an additional column (Thermodynamic state). Additions use this column in cases where the state is not available in the subtitle line.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0c6f45d67005`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.