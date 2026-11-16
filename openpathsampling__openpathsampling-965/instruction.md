# Issue

**Title:** Update ujson pin

## Description

With the [release of ujson 4.0.2](https://github.com/ultrajson/ultrajson/releases/tag/4.0.2), we should be able to relax the pin of ujson without regressing on #922. Since that release is very new, I'm not going to assume that it is in all distribution channels yet, so the pin is kind of complicated (still allowing ujson <2 while also allowing >=4.0.2).

By the next release, I hope we'll be able to just pin `ujson >=4.0.2`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5f9c5455626a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.