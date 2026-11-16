# Issue

**Title:** include region

## Description

I included a new flag -region which can apply rubber bands only to the atoms within the designated regions. One example can be that I know resid 200-300 and 450-500  in my protein is structured, but the rest residues are not. I can use the following command to apply rubber bands only within resid 200-300 and 450-500 among different chains:
martinize2 -f example.pdb -ff martini3001 -dssp -elastic -merge A,B,C,D -scfix -region 200:300,450:500

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4182683bc13e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.