# Issue

**Title:** Robust sort_backbone: detect disconnected selections and avoid infinite loops

## Description

## Problem:
`sort_backbone` can hang indefinitely when the input `AtomGroup` is disconnected (e.g., some backbone atoms were excluded by selection). Currently, the function only checks `n_fragments`, which is insufficient to catch all cases of disconnected backbones.


## Cause:
1. The reliance on `n_fragments` alone does not reliably detect disconnected selections.
2. The `while` loop in `sort_backbone` iterates until the backbone is fully traversed, but if the selection is disconnected, the loop can never complete.


## Solution / Changes made:

1. Added robust validation before the traversal, checking:
    - Each atom’s connectivity degree: 1 for caps, 2 for internal atoms
    - No atom has a degree outside {1, 2}
    - Exactly two cap atoms exist
    - Added final sanity check after traversal (ensure the sorted backbone has the same length as the input)
    - All error messages now report the problematic atoms to help with debugging.

3. Replaced the “dangerous” unbounded `while` loop with a bounded iteration over the backbone length.


Tested with backbone selections missing internal atoms, that now raises a descriptive ValueError instead of hanging.


## PR Checklist
 - [x] Issue raised/referenced?
 - [x] Tests updated/added?
 - [ ] Documentation updated/added? No change needed?
 - [x] `package/CHANGELOG` file updated?
 - [x] Is your name in `package/AUTHORS`? (If it is not, add it!)

## Developers Certificate of Origin
I certify that I can submit this code contribution as described in the [**Developer Certificate of Origin**](https://developercertificate.org/), under the MDAnalysis [LICENSE](https://github.com/MDAnalysis/mdanalysis/blob/develop/LICENSE).


<!-- readthedocs-preview mdanalysis start -->
----
📚 Documentation preview 📚: https://mdanalysis--5113.org.readthedocs.build/en/5113/

<!-- readthedocs-preview mdanalysis end -->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5d48c5cf5f89`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.