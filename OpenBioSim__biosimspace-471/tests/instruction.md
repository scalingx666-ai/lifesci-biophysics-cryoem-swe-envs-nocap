# Issue

**Title:** Fix for ATM centre of mass restraints

## Description

In their current form the COM restraints used by the ATM implementation always apply distance restraints that pull the bound ligand to within 0Å (+- some tolerance) of the centre of mass of the protein. This restraint works fine when the binding site is within the tolerance distance of the centre of mass, but completely breaks down beyond that point, as can be seen in #470 .

This PR adds proper calculation of the starting distance between both ligands and the centre of mass of the protein (which can either be automatically calculated or passed as a list of indices by the user).

This functionality has been tested on the system mentioned in #470 . This is what the system looks like after solvation, prior to any min/eq:
<img width="2584" height="2229" alt="solvated" src="https://github.com/user-attachments/assets/f1a29c92-78d4-4a86-86be-76d2057cda29" />

and this is what it now looks like after being run through the short minimisation/equilibration pipeline shown in the ATM tutorial:

<img width="2733" height="2314" alt="mineq_final" src="https://github.com/user-attachments/assets/596b4973-70b1-485b-9e53-9ed8648eab03" />

.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1c1aded87fe9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.