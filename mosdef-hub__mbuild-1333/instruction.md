# Issue

**Title:** Add `hoomd_nvt` method to `simulation.py`

## Description

### PR Summary:

This adds a new hoomd simulation method we can use on a compound that runs in the NVT ensemble, letting you set a temperature. Note that this also includes the changes in #1332, so we can merge that one first, or close it in favor of this one.

The idea here is to use this in small spurts to help with system initialization, not to run production length simulations or anything like that. 

This also may be a method where we can add a target box parameter that can be used to compress/expand an mBuild compound.

### PR Checklist
------------
 - [x] Includes appropriate unit test(s)
 - [x] Appropriate docstring(s) are added/updated
 - [x] Code is (approximately) PEP8 compliant
 - [x] Issue(s) raised/addressed?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `64c9d56281b9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.