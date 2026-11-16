# Issue

**Title:** Handle when load_model is specified twice

## Description

Currently the config file overwrites the `load_model` argument from the command line, if specified. It would be more intuitive if the command line argument overwrites the config file as this matches the behavior of all other arguments. Now we show a warning if `load_model` is specified twice and the two arguments differ and we ignore `load_model` in the config file.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `39a2a3de6842`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.