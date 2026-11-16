# Issue

**Title:** Format `pyproject.toml`

## Description

This is (almost[^1]) purely a formatting-only change, but it looks more than insignificant so I'll hold off on hero-merging

I wanted to sneak this into #2110 but thought better of that

[^1]:  change is substantive, but a net positive as [that classifier is going away](https://peps.python.org/pep-0639/#deprecate-license-classifiers)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2a66046e0a68`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.