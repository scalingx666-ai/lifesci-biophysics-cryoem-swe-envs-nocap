# Issue

**Title:** add boxes to parsers

## Description

For multiple reasons it would be very useful, when the coordinate parsers would also store the box. I've added this parsing feature to the gro / pdb parser in the least invasive way. The gro solution is not so neat but then again the parser is also entirely out of line with the rest. 

For the PDB parser I stored the box in the parser object itself. In my opinion this would be safer and then we can decide when and if to use it with molecules. Unfortunately this doesn't work for the gro file reader so it then becomes a molecule class variable. I'm open to other suggestions, but would rather have it fast than redoing any major part of the parsers

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `cc49fc2166f0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.