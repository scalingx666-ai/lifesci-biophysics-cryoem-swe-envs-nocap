# Issue

**Title:** Fix creation of Molly.System from AtomsBase structures

## Description

AtomsBase does not define what types to use for position and velocity. This can cause some problems, e.g., ExtXYZ data format causes these. To fix this, you need to convert to defined data structure (`SVector` here).

Also, some checks to coordinates and velocities caused some errors. So, I updated those as well.


There are still some potential issues, like what to do, if velocities are missing. I can add a fix for this too, or do you want to do that in some other PR?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e4d151897bbf`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.