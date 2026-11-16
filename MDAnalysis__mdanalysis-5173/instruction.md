# Issue

**Title:** Fixes #5170: Improve msd_type parsing

## Description

This PR implements enhancement proposed in issue #5170 

## Summary
EinsteinMSD currently lowercases the msd_type string but does not:
- strip whitespace
- validate the type
- handle mixed-case safely

## What This PR Does?

1. Add` .strip()` before `.lower()` to handle inputs like " xy ".
2. Add type validation (raise TypeError when msd_type is not a string).
3. Add new tests to cover:
   - whitespace (" xy ")
   - uppercase/mixed case (" Xz ")
   - non-string inputs

All tests pass locally

<!-- readthedocs-preview mdanalysis start -->
----
📚 Documentation preview 📚: https://mdanalysis--5173.org.readthedocs.build/en/5173/

<!-- readthedocs-preview mdanalysis end -->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `bbcef1bcfaf8`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.