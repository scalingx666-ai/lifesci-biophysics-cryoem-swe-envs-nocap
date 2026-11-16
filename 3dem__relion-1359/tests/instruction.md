# Issue

**Title:** fix: make patch motion groups the sum of the underlying frame data

## Description

I noticed while playing around with motion correcting movies with intentionally inadvisably high fractionation that grouping was not having the expected result. I think there is a bug here where currently a patch's group actually just ends up set to the contents of the last frame in the group, whereas I assume what I have in this pull request was/is the actual intent: sum the contents of the frames within each group/patch?

When I tested this change, grouping gave better/more successful local motion results than the existing code.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e4a4aad06f07`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.