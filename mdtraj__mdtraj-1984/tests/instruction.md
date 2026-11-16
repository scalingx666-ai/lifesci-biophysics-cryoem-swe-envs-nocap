# Issue

**Title:** Feature: Calculate Pi-stacking+T-stacking interactions given aromatic groups

## Description

Hi all,

I wanted to implement a pi-stacking interaction calculator across an mdtraj.Trajectory object. As such, I implemented a (numpy-)vectorized version of the [ProLIF](https://github.com/chemosim-lab/ProLIF) library's implementation of their [pi-stacking interactions](https://github.com/chemosim-lab/ProLIF/blob/742203b794bc39fb9a8161683675079dc12ecb96/prolif/interactions/interactions.py#L284-L370). This implementation focuses on operating across trajectory frames, whereas the prolif implementation parallelizes all of its fingerprint interaction types using python's multiprocessing module.

Pi-stacking is a desirable interaction to view across a simulation. Here's an open issue in MDAnalysis regarding pi-stacking:
 

The implementation I introduce here leaves it to the user to know the atom indices of the aromatic groups in the ligand, as well as the aromatic groups on the protein. It's possible to detect these programmatically with openeye's toolkit, rdkit, etc. ProLIF uses [SMARTS + RDKit](https://github.com/chemosim-lab/ProLIF/blob/742203b794bc39fb9a8161683675079dc12ecb96/prolif/interactions/interactions.py#L310-L313) pattern-matching to do such a thing. Of course, if one wanted to provide a user a basic way to detect aromatic groups for a protein at least, one could find all standard-named atoms on standard AA residues (TYR, TRP, etc.) and detect those. However, I did not want to over-complicate this PR, and I find that proposal / interface to be vague.

This implementation is inclusive of face-to-face pi-stacking and face-to-edge (T-)stacking interactions. I do not delineate in the code which ones are which. This could be added if desired.

This implementation is designed to operate efficiently on a `Trajectory` object. Using a system of approximately 7000 atoms with 1000 frames, the `pi_stacking` function took approximately 0.3s to run, with most of that time (0.23-0.25s) spent calling `compute_distances_core`

I added tests with two systems from the RCSB to hopefully illustrate the capabilities of pi-stacking detection. One system has clear pi-stacking and ambiguous t-stacking ([4BFQ](https://www.rcsb.org/3d-view/4BFQ/1)), whereas the other system has clear T-stacking ([6A22](https://www.rcsb.org/3d-view/6A22/1)). I show that one can tweak the parameters to the `pi_stacking` function to encapsulate the t-stack demonstrated in 4BFQ

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `844675871df3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.