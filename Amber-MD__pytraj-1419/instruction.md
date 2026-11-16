# Issue

**Title:** Install cleanup (mostly for Windows)

## Description

address comments from @multiplemonomials 

# Issue 3: pytraj DLL handling, 
> Pytraj is putting libcpptraj.lib

remove that copying

# Issue 5: 
> Issue 5: conversion of .pyx path to module name. "\\"

done

# 
> libraries=['libcpptraj'],

# Issue 6: include unistd.h
> unistd.zip
This probably goes without saying, but make sure that that file is only in the include path when building on Windows, not on Linux or Mac.

Done.

# Also
- Always using `setuptools`

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3f9a8d6395c6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.