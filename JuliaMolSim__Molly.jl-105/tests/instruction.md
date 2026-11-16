# Issue

**Title:** Started Muller-Brown potential

## Description

Hello,

Just started working on the Muller-Brown (MB) potential as a first project so this is definitely a draft. I had some questions about how this potential should be incorporated into Molly. MB is not like any of the other potentials in Molly at the moment, it is only 2D and is not a pairwise interaction (just a surface). Assuming I can figure out how to integrate MB into Molly I will add test coverage in a future commit.

Questions:
1. Other interactions are typed as SpecificInteraction or PairwiseInteraction, MB potential does not fit into either of these. Would I need to create something new (e.g. PotentialEnergySurface)
2. Because it is only 2D how do I make sure this potential is operable within the framework of Molly that is mostly built around pairwise interactions or bonded interactions (e.g. 2 atoms get passed to the potential() function). I have also not implemented a force() function yet. I do not really see how to make the interface used by other potentials amenable to MB.
3. I'm pretty new to Julia so I have a lot of uncertainty around the typing and in general how you go about a large code base without OOP. My brain is stuck thinking in abstract classes and interfaces so any pointers are appreciated.

Thanks,
Ethan

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b172d0d8ca3d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.