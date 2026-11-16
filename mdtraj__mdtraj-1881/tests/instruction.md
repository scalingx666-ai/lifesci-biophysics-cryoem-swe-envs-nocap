# Issue

**Title:** CI macos-latest fix

## Description

Just in case any of the maintainers are wondering, GitHub Action runner images for `macos-latest` has shifted to `macos-14-arm64` a few days ago (from `macos-12`, which is x86-64). That's probably causing the fails in the scheduled CI.

Changing from `macos-latest` to `macos-13` should fix this for the time being, until the arm64 workflows are ready. `macos-13` is the last "standard" (i.e., free) x86-64 runner image from GitHub.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `da52c9c364e6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.