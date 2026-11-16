# Issue

**Title:** pin numpy>=1.25 at run time, numpy>=2.0 at build time for numpy C-API

## Description

Proabably should've done this when we dropped `oldest-supported-numpy` and added numpy 2 support but here it is. 

This ensures numpy 2 is used during build time so the compiled files are compatible with numpy 1 and 2. numpy>=1.25 is required at run time so the C-API exporting works.

https://github.com/scipy/oldest-supported-numpy

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d333d360577f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.