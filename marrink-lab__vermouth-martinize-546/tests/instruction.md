# Issue

**Title:** Replace deprecated pkg_resources with new importlib

## Description

So, `pkg_resources` is deprecated (https://setuptools.pypa.io/en/latest/pkg_resources.html), so here I change it for `importlib` as recommended.

See also https://importlib-resources.readthedocs.io/en/latest/migration.html

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `72766a297caf`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.