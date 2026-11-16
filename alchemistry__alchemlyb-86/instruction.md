# Issue

**Title:** gmx parsers now drop duplicate columns

## Description

Some of our datasets, such as [`alchemtest.gmx.benzene`](https://github.com/alchemistry/alchemtest/tree/master/src/alchemtest/gmx/benzene), feature duplicate columns in their data files. Previously this was handled after initial parsing with `pandas.read_csv`, but as of the latest `pandas` release this function disallows duplicate column names on entry.

We now mark duplicate column names before entry into `pandas.read_csv`, and we filter these duplicate columns from the resulting dataframe after this initial parsing.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `710a522cc38b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.