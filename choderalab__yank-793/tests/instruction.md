# Issue

**Title:** Add new Regions API to Topography

## Description

Add the ability to add and manipulate arbitrary regions to the Topography object. These are more general than the alchemy based alchemical regions and are only subsets of atoms.

Generalizes string selection handling with a future PR to handle SMARTS strings (as marked by TODO's). This PR does not address those so its a smaller chunk to review.

Adds a handy multitmethod technique to reduce cyclomatic complexity we may be able to apply elsewhere.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `92ef7c770c9b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.