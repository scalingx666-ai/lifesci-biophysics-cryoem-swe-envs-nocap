# Issue

**Title:** XYZwriter also for moleculekit object

## Description

I noticed that the moleculekit object give an error during the xyz_writing.  Here I propose an update version that it's able to work with both npzmol and moleculekit object. 
Key points:
- [x] `element` attribute to the npzMolecule object using moleculekit.periodictable so we have the same attribute as in moleculekit object;
- [x] new name for the `xyz_writer` function with relative documentation;
- [x] now the xyz_writer function use directly the `mol.element` to write the element symbol in the xyz file;

The error which I'm referring to fix: 
![image](https://github.com/torchmd/torchmd/assets/101815262/9e8d4f24-85d8-4a02-8eb8-db5946ad38bc)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1bad86ec08ce`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.