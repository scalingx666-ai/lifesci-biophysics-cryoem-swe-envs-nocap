# Issue

**Title:** Classes -- Support for atoms classes

## Description

# Atom classes

Until now, Molly loaded force field definitions solely based on atom types, and did not support definitions based on atom classes. This PR aims to extend Molly in that sense. In fact, an attempt at this had already been made in the previous PR, but the implementation was too naive.

## The problem with wildcards

The way atom classes work, in general, is as a supra category for atom types, this is, several atom types can be grouped in a single atom class. This allows to define interactions for several atom types at once, if atom classes are used as the key. Atom classes, as atom types, can be explicitly defined for an interaction or said interaction could be keyed by a wildcard (or several wildcards) instead. Wildcards imply that any class (and therefore type) can appear at a given position of the interaction. It is easy to see, then, how this can lead to an explosion of time complexity if the algorithms used to materialise the interactions are not built in a smart way (which I definitely didn't do in the last PR); specially for definitions of interactions with a lot of atoms involved in them (torsions), even more so if wildcarded classes are present.

## The fix to the problem

### Force Field loading

The way the forcefield is loaded and, particularly, the way the interactions are indexed for posterior materialisation has been reworked. It tries to follow the way OpenMM deals with this problem. Until this point, all the interactions were indexed explicitly by all of the atom types involved in them with the `bond/angle/torsion_types` fields of the `MolecularForcefield` struct. Instead of this, now the interactions are indexed only by their "anchor" atom (be it a type or a class) for example, the central atom of a given angle. This is handled by the newly introduced `Bond/Angle/TorsionResolver` structs, which carry all the necessary information for the efficient indexing, resolution and materialisation. These structs are present in the `MolecularForcefield` as the `bond/angle/torsion_resolver` fields. 

### System setup

At the time of actually materialising the interactions for a system read from a structure file, the anchor atoms are queried against the interactions indexed in the forcefield. A list of potential candidates is built, so the interaction is only compared to a subset of all the possible interactions present in the force field; and then the interaction is resolved using the full interaction signature (the atoms involved) found in the structure of the system. Multiple potential matches are resolved by a priority score, which favours types > classes > wildcards. When a match is found, it is added to a caching dictionary so the resolution does not have to be performed again if the same interaction signature is found several times in the structure, further improving performance.

## Improper torsions reordering

Improper torsions carry with them some reordering rules which, unfortunately, are not shared between different parameterisations. I am confident that the amber torsion ordering is properly implemented and works the intended way. On top of this, I have also added logic to handle charmm, smirnoff and what OpenMM calls the "default" ordering. I have followed the rules implemented in OpenMM to handle this cases, but as at the moment we lack a way to properly deal with force fields other than Amber, this paths have not been properly tested, which is something that must happen in the future.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a34f677d269e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.