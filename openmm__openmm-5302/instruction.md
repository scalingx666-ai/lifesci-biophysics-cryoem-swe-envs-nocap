# Issue

**Title:** Fix `AmoebaMultipoleForce::ForceInfo` particle groups used for molecule detection and atom reordering

## Description

This PR fixes `AmoebaMultipoleForce::ForceInfo` so that molecule detection in `ComputeContext::findMoleculeGroups()` sees complete connectivity.

#Problem

`AmoebaMultipoleForce::ForceInfo::getParticlesInGroup()` currently returns only the covalent map entries, but omits the central particle. `findMoleculeGroups()` interprets each returned particle group as a connected chain and builds molecular connectivity from consecutive entries in that vector. Omitting the central particle can therefore split a real molecule into multiple connected components.

This becomes problematic on CUDA because `reorderAtoms()` reorders atoms by molecule. If molecule detection is wrong, atoms can be reordered across molecule boundaries, while precomputed exclusion tiles and covalent scaling tables still assume the original internal molecule structure.

 

#Changes
- Replace hard-coded `7` with `AmoebaMultipoleForce::CovalentEnd`
- Include the central particle in `getParticlesInGroup()`

#Result
This restores correct molecule grouping and fixes the observed CPU/CUDA mismatch in our test case.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `fc21fd1923ae`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.