# Issue

**Title:** Itp reader

## Description

This is the first version of an itp reader to tackle several issues we had with the ff-reader being unable to deal with certain GROMACS itp specifics. The itp reader could either form a base class for the ff reader. It could also be used to perform automatic checks of files written by vermouth. The itp reader has the following differences to the ff-reader:

- it deals correctly with VS supplied using the index method solving issue #235 
- it supports `#ifdef` statements in the correct places otherwise raises an IO error 
- it generates nodes based on atom indices thereby allowing for multiple atom-names in one block
- it has parsers for all GROMACS interactions
- it does not create edges from interactions at the moment; we can check if we would want this and which interactions to use and how to deal with VS
- it probably can parse various tokens from the ff format, but is not intended to. So these are unchecked. Because interpretation of these special tokens happens so deep in the base_parser it was much easier to just leave this in. 
- worth mentioning: it actually does split dihedrals into impropers and regular dihedrals 

At the moment it cannot deal with:
- `#include` statements
- `#define` statements as used in the topology to indicate, if something is defined. 
- `[settles]` directives are not read at all

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2a72ce6427c1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.