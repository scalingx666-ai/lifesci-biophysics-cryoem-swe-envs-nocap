# Issue

**Title:** ABFE workflow will not read dHdl file if no TI estimator is choosen

## Description

If not explicitly set, AMBER will not generate the dHdl file. (I don't know if the u_nk will be written by default but I got a complain that workflow cannot be used to process their amber output, which doesn't have dhdl data). Currently, the ABFE workflow will attempt to read both dHdl and u_nk. This PR will make it such that if no TI estimator is chosen, the workflow will not attempt to read the dHdl.

I have also removed the ignore_warnings in the read file stage as maintaining it and having it to be clever enough to solve all the dumb user issues seems to be too much.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `eb698c368ce3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.