# Issue

**Title:** pdb_validate outputs to sys.stdout

## Description

Implements changes proposed by @brianjimenez in #43 

Because this is a design related implementation I would like to hear from @JoaoRodrigues, please decide whether or not you agree with the proposal.

In case you do, here is the PR already.

I have not run the tests locally. I will wait for the CI to do it for me :smile:

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b39fb96c24e0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.