# Issue

**Title:** Allow pair styles with additional options

## Description

Pair styles requiring additional settings (everything with pair_style style args, f.e. pace recursive or  lj/long/coul/long cut off 2.5) caused problems when calling hybrid pair_style because the whole line was added.

This pr splits pair styles into the style and args, writing both when directly calling the pair style, but only the style when used in hybrid/scaled or computes. Therefore the user input is split() into styles and args and a pair_style_with_options property is added to the Calculation class that reconstructs the full string.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4f7ec5a3808c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.