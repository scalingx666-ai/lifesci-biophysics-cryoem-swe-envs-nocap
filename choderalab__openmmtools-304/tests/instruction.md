# Issue

**Title:** Make libyaml optional

## Description

Not all of us have pyyaml installed with libyaml support. For those who don't, this uses the old pure python yaml as fallback. The (much faster) `CLoader` and `CDumper` are still preferred.

Uses the approach recommended at: https://pyyaml.org/wiki/PyYAMLDocumentation

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1c229362520e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.