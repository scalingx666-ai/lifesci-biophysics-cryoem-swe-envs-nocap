# Issue

**Title:** Skip some tests if unpackaged files not present, make some files path-y-er

## Description

The tests currently make some unsafe assumptions about files present relative to the Python package. For example, this does not necessarily point to the root of the repository, since many files are not packaged:

https://github.com/openforcefield/openff-toolkit/blob/5ffcb58714c93143d8c00841c00e73f46a6c296f/openff/toolkit/_tests/test_links.py#L7-L9

From this, peeking at the `README.md` file (or looking for an `examples/` directory) can introduce some surprises:

```python
In [1]: import pathlib, os

In [2]: from openff.toolkit._tests.test_links import __file__

In [3]: os.system(f"head {pathlib.Path(__file__).parents[3] / 'README.md'}")
# autoflake

[![Build Status](https://github.com/PyCQA/autoflake/actions/workflows/main.yaml/badge.svg?branch=main)](https://github.com/PyCQA/autoflake/actions/workflows/main.yaml)

## Introduction

_autoflake_ removes unused imports and unused variables from Python code. It
makes use of [pyflakes](https://pypi.org/pypi/pyflakes) to do this.

By default, autoflake only removes unused imports for modules that are part of
Out[3]: 0
```

This change simply has fixtures return empty lists when it looks like tests are being collected from a packaged path (as distinct from a local path, where the full contents of the repository may be present).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3c56d29909fb`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.