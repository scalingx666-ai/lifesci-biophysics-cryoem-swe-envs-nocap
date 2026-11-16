# Issue

**Title:** Constrain `cython=0.29`

## Description

Band-aid for #1800, which should be followed-up by a proper fix.

Version 3.0 (!) was released, but the current source code is only compatible with versions 0.29.x

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a1e54569caf4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.