# Issue

**Title:** SHAKE constraint implementation for bonds

## Description

I accidentally closed the previous PR (by merging it back into my old repository).

I've made most of the changes requested in the comments of the previous PR by @jgreener64 .

I kept the name of the master function that applies all the constraints as `run_constraints!` since I felt `apply_constraints` would be confusing when I've used `apply_constraint!` for a single constraint. If there's no better nomenclature I don't mind using `apply_constraints` too. 

Copying over the list of remaining tasks: 

- [x]  Timing simulations with and without constraints
- [x]  Add a unit test with SHAKE
- [ ]   Updating docs.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `380287bba2a5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.