# Issue

**Title:** Identify and fix edge case in ISMAGS symmetry detection

## Description

Fabian found an edge case in the ISMAGS symmetry detection. This should fix it. I'll also see about pushing a fix to the networkx version at some point.
Note that I'd like this reviewed by both of you.

```
5 - 4 \     / 12 - 13
       0 - 3
9 - 8 /     \ 16 - 17
```
Assume 0 and 3 are coupled and no longer equivalent. At that point {4, 8} and {12, 16} are no longer equivalent, and neither are {5, 9} and {13, 17}. Coupling 4 and refinement results in 5 and 9 getting their own partitions, *but not 13 and 17*. Further iterations will attempt to couple 5 to {13, 17}, which cannot result in more symmetries?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `45ab4c8397ba`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.