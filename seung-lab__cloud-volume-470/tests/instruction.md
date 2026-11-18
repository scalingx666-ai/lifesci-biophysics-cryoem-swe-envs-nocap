# Issue

**Title:** redesign: Skeletons respect SWC structure (more)

## Description

The goal is that when a skeleton passes through `Skeleton.from_swc(...).to_swc()`, you might not get exactly the same SWC, but the structure will be retained.

This is important to prevent "gottchas" from surprising new users.

We accomplish this by relying more heavily on the edge graph than on vertices for several calculations. This also improves the robustness of the Skeleton.equivalent call by using an actual graph algorithm to check for a match. I'm lothe to add new dependencies, but this one seems important and reimplementing would be expensive.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `bde393b48031`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.