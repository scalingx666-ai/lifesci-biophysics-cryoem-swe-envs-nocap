# Issue

**Title:** Water bridge 2nd Order implementation

## Description

Changes made:

- Addition of code for 2nd Order water bridges
- Implementation of input variable order

we would need to see if we make the order input then in the Fingerprint class and have a default, currently being 1
that the user can then input in the Fingerprint class.

As for the higher water bridge orders we would need to see how we do the recognition of interactions within the 
water-water analysis multiple times. Maybe through some looping like some step wise matching

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `72e48549140f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.