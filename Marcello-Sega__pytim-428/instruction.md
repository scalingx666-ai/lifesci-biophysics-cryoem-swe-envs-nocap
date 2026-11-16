# Issue

**Title:** Add density cutoff argument to Willard Chandler

## Description

The Willard Chandler method takes a parameter _c_ that defines the density to use to describe the isosurface. In Pytim, this defaults to the average of the minimum and maximum density. However, sometimes one might want to use a different value, so I've just added an argument to the `WillardChandler` class so we can!

I also deleted `.travis.yml.`, which you can undo if you'd like. I wasn't able to clone the repository on Windows because the file name is invalid. I suspect it should be `.travis.yml` (without the trailing `.`), so it probably needs renaming anyway.

Please make sure that your pull request satisfies the following requirements:

#### If you have written a new method in one of the available modules

  - [x] Document it with a docstring
  - [ ] Add a short, deterministic test in the docstring
  - [x] Explain briefly what the patch is about in the pull request

Please let me know if you want me to add another test to the docstring; it might make it a bit lengthy.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e975d76f7a74`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.