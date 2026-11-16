# Issue

**Title:** Crosslinking Simplified

## Description

### PR Summary: Aims to modularize and simplify the crosslinking proposed in #1324 


In order to optimize crosslinking speeds, have refactored the crosslinking function in `path/crosslink.py`.

Overall changes:
1. Crosslinking is actually a problem with many different approaches. The actual mechanisms of how this can be done will differ significantly based on the geometry of the crosslinker. If bonds are just being added directly across backbones, that's much different than a 4 site crosslinker where two crosslinker sites bond to one backbone, and then the other two to their own individual backbones. In order to facilitate better optimization and better case-by-case usage, the crosslinking function now calls individual working functions in the main crosslink loop, using crosslinkKey to distinguish these cases.
2. Only direct crosslinks and single site crosslinks (i.e. crosslinks with 1 site bonding to two backbones) have been setup.
3. Overlaps and distance checks are performed with freud or scikit.cKDTree for optimized placement validations. 
### PR Checklist
------------
 - [ ] Includes appropriate unit test(s)
 - [x] Appropriate docstring(s) are added/updated
 - [x] Code is (approximately) PEP8 compliant
 - [ ] Issue(s) raised/addressed?
 - [x] Support direct crosslinks
 - [x] Support one-site crosslinks
 - [ ] Support two-site crosslinks

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `02ba61de423e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.