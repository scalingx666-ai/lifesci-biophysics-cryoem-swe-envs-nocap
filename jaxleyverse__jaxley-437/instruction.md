# Issue

**Title:** more options for plotting

## Description

I have continued #432 and added two new options:

- plotting the true traced morphology (i.e. including radii of every traced point not just for each comp) 
![image](https://github.com/user-attachments/assets/253e5d3d-fab4-41c0-bd2f-2c8f873ed5e3)

- and allowing to plot all 3 axes in a 3d plot
![image](https://github.com/user-attachments/assets/d1c226a0-8fc0-4855-8b3b-e341f1cd801d)

Also added this to the tests. They are passing.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `dfd5087728bf`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.