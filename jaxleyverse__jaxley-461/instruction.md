# Issue

**Title:** fix units of compute_current

## Description

This is a tricky PR as it will impact users which ported channels themselves. I hope that this is not many people yet, so now is the time to still do it.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `61c5c276c867`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.