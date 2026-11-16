# Issue

**Title:** Expose `normalize` option to `writecube`

## Description

#### If you have written a new method in one of the available modules

  - [x] Document it with a docstring
  - [x] Add a short, deterministic test in the docstring 
  - [x] Explain briefly what the patch is about in the pull request

Currently, pytim normalises the calculated density field by default when writing to a cube. This just exposes the `normalize` flag to the `WillardChandler.writecube()` method so you can disable that behaviour -- makes it much easier to find the right values in visualisation software.

This is a redo of #436 with an isolated branch.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `fd045bd3c0be`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.