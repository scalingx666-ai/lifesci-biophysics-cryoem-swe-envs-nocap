# Issue

**Title:** Replace lark-parser with lark in the environment files.

## Description

This was originally added as part of #574, but that PR might have to sit open for a while until we want to drop 3.9 and support 3.13, so I figured I'd add the lark related changes to a separate PR. This should get rid of one deprecation warning, plus makes sure we're using updated versions of lark.

`lark-parser` was the old conda environment name for the lark package, it's now just `lark`, but it's the same package.

I'll open an issue in the foyer feedstock so we remember to change it there as well.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ad0b4d33db8e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.