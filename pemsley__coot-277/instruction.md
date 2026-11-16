# Issue

**Title:** made tests pytest compatible

## Description

The conftest.py will automatically find and run unittest tests from Test*.py files by using a subprocess.

It parses Test*.py files to find the names of test classes and methods without importing them, because importing would fail due to the coot dependency.

CootUnittestItem:

-   takes the name of a test class or method.
-   Generates a Python script that runs a single unittest test
-   Spawns a coot subprocess with --no-graphics --script
-   Reads results from a JSON file
-   Reports pass/fail back to pytest


Created CootUnittestFile for each test method found

Added pytest_collect_file hook which intercepts Test*.py files and returns to collector instead of letting pytest try to import them

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0bce2660b95d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.