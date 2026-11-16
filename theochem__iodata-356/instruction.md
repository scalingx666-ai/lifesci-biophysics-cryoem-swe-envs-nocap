# Issue

**Title:** Do not use iodata as variable name

## Description

Fixes another task on the list of #191.

Having `iodata` as a variable name is bad because this is also the package name. This fixes it in a way that is consistent with the rest of the IOData code, e.g. as in the file format modules.

I will YOLO-merge this on Friday, June 5, unless reviewed earlier.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8cf113273916`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.