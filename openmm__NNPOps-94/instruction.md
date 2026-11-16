# Issue

**Title:** Support for pytorch 2.0

## Description

This PR is to start working on making NNPOps compatible with pytorch 2.0 and torch.compile

- [x] Ensure the workflows and tests work for pytorch 2.0 (wich requires CUDA 11.8)
- [ ] Make operations compatible with torch.compile()
   Hopefully this will not require a lot of work, but reducing the number of graph breaks torch.compile will introduce will probably be more challenging. I reckon avoiding graph breaks is really similar to making stuff compatible with CUDA graphs. 
- [ ] Write tests for compiled versions
       In principle compiled functions/models should be completely equivalent to the uncompiled versions, but I have seen this not being the case (granted, torch2 was still a beta)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c5b12ba890e8`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.