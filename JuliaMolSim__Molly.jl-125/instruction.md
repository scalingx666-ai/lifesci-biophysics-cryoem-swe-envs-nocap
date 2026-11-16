# Issue

**Title:** Crystals v2

## Description

Don't even know what happened in the last branch. Unbroke whatever merge issues I had from pulling the CUDA branch.

Things to remember:
- Possible type conflict between SimpleCrystals.Atom & Molly.Atom
- Class for replica system too?
- Float promotion?
- How to take extra Atom params like sigma and epsilon? (having things like this in the Atom object makes it impossible/annoying to actually use the AtomsBase interface to load a system from another package and simulate it).

- Part of me feels that Atom data like this should be stored as a dictionary separate from the Atom struct and not per atom. Probably not a huge performance hit for small systems but I think it could be a lot of extra memory for really large systems. Would be a pretty big change as most interaction classes rely on each atom having its own data stored internally.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5cf4d2965078`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.