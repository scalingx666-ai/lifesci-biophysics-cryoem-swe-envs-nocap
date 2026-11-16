# Issue

**Title:** Fix regressions in the cell structure

## Description

Description of changes:
- properly handle ghost particle edge case in the LB particle coupling code
   - since 5.0.0, ghost particles could accidentally still track a deleted particle during LB particle coupling
- removed particles are now also removed from the cache
   - before, they would only be removed from the cache at the next particle resort
- improve code coverage

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9f96ff4e5c4e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.