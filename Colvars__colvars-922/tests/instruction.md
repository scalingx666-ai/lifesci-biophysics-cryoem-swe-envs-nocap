# Issue

**Title:** Step up language requirements to C++17

## Description

All backends already require C++17, with the exception of VMD.

This PR makes the necessary changes to be able to patch VMD (1 or 2) to enable C++17 as part of the testing.  I have tested both offline.

Implementation note: because the two VMD versions are now diverging, I've switched from a diff file to sed commands for editing VMD's `configure` script.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5eb5bcdca2d6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.