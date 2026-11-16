# Issue

**Title:** mmCIF reader and writer

## Description

The standard PDB archive format since 2014 has been mmCIF: https://www.iucr.org/resources/cif/spec/version1.1/cifsyntax

This PR (currently work in progress) adds a reader and writer of the mmCIF format. It reads mmCIF files into the same hierarchical model as PDB files, or you can just access the mmCIF dictionary directly. The same API as for PDB files is maintained for reading and writing.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `675254dbb5b6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.