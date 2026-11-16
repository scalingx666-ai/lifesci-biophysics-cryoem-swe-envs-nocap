# Issue

**Title:** Fix MISTIS minus ensemble not forbidding other states

## Description

The minus interface as set up by the `MISTISNetwork` didn't forbid the trajectory from entering other states during the excursion from the state. This would occasionally manifest as a failure of the sanity check when saving, stating that the trajectory for some replica did not satisfy the ensemble (because the trajectory in the TIS ensemble after a replica exchange had frames in the other state). 

Typically, this meant that the minus trajectory had to cross to the other state and then *cross back*, so it was extremely rare, but I found it when working on a simple model that isn't actually rare enough to really need TIS to get the rate. @gyorgy-hantal: I think you encountered this problem once. This is the explanation.

This fixes it -- everything was in place to forbid other states (and used in MSTIS); just needed to add it to MISTIS. Also adds a test to prevent regression.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f403459403b6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.