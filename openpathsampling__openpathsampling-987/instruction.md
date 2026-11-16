# Issue

**Title:** Switch notebook tests to nbval

## Description

We currently run several notebooks as tests -- mostly system-scale smoke tests, though also some more unit-level testing. These tests are run using a tool that @jhprinz wrote, [ipynbtest](https://github.com/jhprinz/ipynb-test).

Unfortunately, ipynbtest hasn't been maintained for a quite a while. Additionally, after @jhprinz developed ipynbtest, another tool, [nbval](https://github.com/computationalmodelling/nbval), became a widely used pytest plugin for using Jupyter notebooks in testing.

With no one to maintain ipynbtest, I think we need to switch to nbval. This PR does that. I'm sad about this, because I think ipynbtest is a nicer tool to use (I like the way it reports Markdown headings as a progress indicator while running.) However, this is now the only thing blocking us from testing on Python 3.8 and 3.9, so the time has come.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d32c198fc014`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.