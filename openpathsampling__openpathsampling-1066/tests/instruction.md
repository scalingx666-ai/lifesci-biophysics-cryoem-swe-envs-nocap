# Issue

**Title:** Clear old ExternalMDSnapshots during `iter_generate`

## Description

This clears cached details from snapshots that have a `clear_cache` method (namely, `ExternalMDSnapshot`s) after the partial trajectory has been yielded to back to the `generate` function.

OPS currently requires that the entire trajectory fit in memory; this relaxes that requirement for users of the Gromacs engine. This is in response to a user-reported problem that might have been caused by this.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ae033d580a13`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.