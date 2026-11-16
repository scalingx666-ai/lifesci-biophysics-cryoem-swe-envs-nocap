# Issue

**Title:** Fix Python 3.12 compatibility

## Description

The `distutils` package is deprecated and no longer available in Python 3.12. We only used the `strtobool` function from `distutils`. This PR adds a `strtobool` replacement to `iodata.utils` and changes the imports from `distutils` by imports from our own `utils.py`.

I've also added `.envrc` to `.gitignore` because it is a local file I'm often using to manage different software envs.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6b9c6e0e94a0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.