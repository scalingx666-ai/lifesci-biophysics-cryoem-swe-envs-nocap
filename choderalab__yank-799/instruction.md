# Issue

**Title:** Add subset selection to Topography

## Description

Adds the ability to filter atoms in the `select` function on a first pass, making the `selection` a relative choice. This is a required step for selecting SMARTS strings so we are only selecting parts of the topography first.

This relies on recursion through the `select` function itself. Open to suggestions for other test cases to include.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3bae8e4ed417`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.