# Issue

**Title:** Add JH to author lists, extra reference

## Description

I've also reordered the middle authors as indicated. A bunch of accented letters were added in the process. I hope it's Unicode all the way down... If that causes trouble in the PDF build, I'll switch to LaTeX escape sequences.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `bc3b7fb2a48b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.