# Issue

**Title:** Avoid multiple forces running on the worker thread at once

## Description

.  CustomCPPForce and PythonForce usually run their calculations on a separate worker thread.  If a system contains two of these forces it can lead to a deadlock.  This disables the separate thread if two or more of these forces are present.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `13c568d08d5a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.