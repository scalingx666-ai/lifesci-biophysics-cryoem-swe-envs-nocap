# Issue

**Title:** Fix module.py

## Description

- [x] `compute_losses` output return a dict with "y" and "neg_dy"; these 2 variables are initialized as float: 
	- if "derivative" is set to false or "neg_dy" not in batch, the neg_dy output is a float and .detach() function called in step give an error;
	- if "y" not in batch, "y" is a float (0.0) and the detach function called in step give an error; 
 According to the old version of the code I added a check for `neg_dy_weight` and `y_weight `
- [x] wandb error:
	- if you want to train with just "y" or "neg_dy", wandb should visualize only the relative metrics;
	- remove same metrics with different name (redundant), I modified `ModelCheckpoint()`;

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4e249109c0d4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.