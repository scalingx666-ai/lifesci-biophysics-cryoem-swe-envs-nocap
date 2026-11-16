# Issue

**Title:** Dashboard improvements

## Description

Most widgets are now ttk. A few still don't work well on high-dpi displays (comboboxes and some text boxes), but that can be improved incrementally - and harmonized with the other well-maintained plugins. That can happen in the VMD2 plugins repo when it's setup.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `aa5d60ca320c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.