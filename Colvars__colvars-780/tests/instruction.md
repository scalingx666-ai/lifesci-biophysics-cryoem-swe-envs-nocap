# Issue

**Title:** [RFC] Add an option to parallelize the inner loops instead of CVCs

## Description

As discussed in  this PR aims to provide the users an option (`smp inner_loop`) to parallelize the inner loops.

Currently this is only in early stage so I mark it as a draft PR.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `549a08fc9e45`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.