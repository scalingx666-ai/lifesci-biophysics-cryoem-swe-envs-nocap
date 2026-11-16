# Issue

**Title:** Doc fixes

## Description

I noticed that the gravity example should have the opposite sign, so here's a quick PR fixing that and a few typos. I also added a codeblock on how to create the SIR plot.

I have an intermediate commit that takes all the examples in the docs and puts them in an `examples/` directory, but I couldn't figure out how to get Documenter to import the blocks of code from a different file, so I deleted the directory in the latest commit.

Basically, if you would like compilable examples in a separate directory, I can revert the latest commit to make them available. Otherwise, I can squash everything as is.

Also: no pressure to merge this if you feel there is not enough substance to it!

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `21be9f823e82`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.