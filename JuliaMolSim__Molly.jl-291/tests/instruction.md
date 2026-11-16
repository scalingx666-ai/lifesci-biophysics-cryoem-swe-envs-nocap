# Issue

**Title:** Detect carboxylate oxygens for mbondi3 from bonds

## Description

GBn2 uses mbondi3 radii, which give carboxylate oxygens a radius of 1.4 Å instead of 1.5 Å. Molly finds these atoms by checking for the atom type name.

This only works for force fields that call the type `O2`, such as ff99SBildn. In `protein.ff19SB.xml` the types are prefixed (`protein-O2`), so no oxygen matches. Every carboxylate then keeps 1.5 Å. Carboxylates on non standard residues are missed in the same way.

Carboxylate oxygens are now found from the bonds, with the same rule as OpenMM's [`_is_carboxylateO`](https://github.com/openmm/openmm/blob/master/wrappers/python/openmm/app/internal/customgbforces.py): an O bonded only to a C that has three bonds, one of them to another O.

Note: this matches carboxylic acids as well as esters. I'm not really sure whether this is the correct way to identify carboxylates to be used in GBn2.
I decided to follow the implementation by OpenMM, but it should not be too hard to identify whether there is a charge or not. I am not too expert in this, but clearly the OpenMM implementation and the Amber implementation are different; I don't know how much of a difference it makes. However choosing one of these methodologies at least make the results consistent within Molly when switching force fields.
Also, I'm not sure if there are cases in which `O2` could be defined with an oxygen atom bonded to something which is not a carbon atom.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6ffe9903c65c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.