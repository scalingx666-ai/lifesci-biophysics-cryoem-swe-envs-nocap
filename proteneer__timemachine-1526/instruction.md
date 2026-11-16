# Issue

**Title:** Tracks water sampling proposals in run_sims_hrex

## Description

* Enables tracking of acceptances of water moves in HREX
* Adds plotting code to plot acceptances by state

## Example Water Sampling plot
Note that the total number of proposals in each state is in the title, as the ratio of acceptances to proposals tends to be ~ 1 in a million proposals. The example plot is taken from the tests where the acceptance rate is much higher. 
![example_ws](https://github.com/user-attachments/assets/4009f9cd-26dc-4062-bd06-df3fedd2c0c4)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `544a37c2ad80`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.