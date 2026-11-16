# Issue

**Title:** Fix 354

## Description

From #354, which this fixes, tested on 2zwb:

> Element D is unknown to make_bonds, which results in very strange "modifications" that have no anchor, since they get no bonds. Fixed by teaching make_bonds about 'D'. A better solution might be to check for the absence of anchors when canonicalizing modifications?

Note that I took the same value for the radius as for H, which is probably a little bit too big.

> The real issue: if a residue contains 2 (or more) modifications, and the first is not known. Once the first modification gets IDed as 'unknown' the relevant atoms get removed from the molecule. When the second modification comes around the processor tries to create a subgraph of the complete residue (based on bookkeeping done earlier), resulting in the KeyError. Fixed by keeping track of which atoms got removed.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `71a7711025fd`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.