# Issue

**Title:** Fix #177 with a --version output containing only a semver string

## Description

I tested in a fresh conda environment that the output of `topaz --version` now matches something accepted by `semver.VersionInfo.isvalid()`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `20fb49f1e120`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.