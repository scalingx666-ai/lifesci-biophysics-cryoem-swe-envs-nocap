# Issue

**Title:** remove stdlibc++

## Description

Seems work fine with

```
export MACOSX_DEPLOYMENT_TARGET=10.14
$ sw_vers 
ProductName:	Mac OS X
ProductVersion:	10.14.3
BuildVersion:	18D109
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `84f9b5736d86`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.