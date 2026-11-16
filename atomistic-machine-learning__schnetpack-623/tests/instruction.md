# Issue

**Title:** Add tmqm dataset

## Description

Together with Stefaan I wrote a loader for the [tmQM](https://github.com/bbskjelstad/tmqm/tree/master) data set.

In line 192 we made a setup --analogous to the materials project dataset code-- to pass a string as a property (usually an identifying code of the molecule in a data base).

I tested this locally with a script and it worked. What is missing is a way to do it with yaml from the CLI.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `17c9a87ef61b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.