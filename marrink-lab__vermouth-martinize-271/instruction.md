# Issue

**Title:** First round of optimizations

## Description

This PR reduces runtime from >6 minutes to <1 minute on my laptop, using the protein from #256. 
Currently the slowest thing is the isomorphism in do_mapping. We can probably do something clever there as well, by checking whether any of the mappings cross residue boundaries, and simplifying the problem from there (per residue isomorphism, or even mapping by atom name). But that's significantly more work.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `cd33cd0a1c50`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.