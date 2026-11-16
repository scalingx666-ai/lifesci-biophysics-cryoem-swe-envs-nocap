# Issue

**Title:** 3D visualization patch for unkekulized mols

## Description

Hi @cbouy 

This is the patch for 

## Problem:
For the 3D visualization, visualizing the cartoon for the protein requires a step of removing all hydrogens before converting to PDB Block. 
https://github.com/chemosim-lab/ProLIF/blob/b016f0917f62fc9f5396b62bb21d678a8ee0cbb7/prolif/plotting/complex3d.py#L509-L510
Since the current version is without sanitization, some molecules cannot be converted into PDB block properly () and leads to a "unkekulize" issue. It depends on the topology of the molecules and it can occasionally happens.

<img width="644" height="181" alt="Screenshot 2026-04-24 at 17 25 58" src="https://github.com/user-attachments/assets/9907410d-3456-46d4-88c0-70ee0fe28fc1" />

## Solution:
My solution here is to add a new parameter `santitize_vis_mols` to allow the users to switch on or off (also options for either protein or ligand) the santization during the stage of the visualization. I believe this can solve most of the cases.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `88d546c89cd1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.