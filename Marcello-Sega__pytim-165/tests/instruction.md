# Issue

**Title:** Documentation

## Description

Fixed a bunch of formatting issues and typos in the docstrings.

NOTE:
when writing docstrings:
:param int foo:        -- OK
:param int bar     :   -- NOT OK (not recognized as a parameter because of the spaces before the colon)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f31bd051a312`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.