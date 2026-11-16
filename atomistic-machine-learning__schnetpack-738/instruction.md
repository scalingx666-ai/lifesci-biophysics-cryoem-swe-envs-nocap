# Issue

**Title:** Sh/petnames

## Description

Added a new `OmegaConf` resolver that generates unique model directories with cute petnames instead of random strings.

Example use:
`run.id: ${uuid:1}` -> `runs/0b504ffc-dd84-11ef-b113-7e8ae1cdf899`

 `run.id: ${petname:1}` -> `runs/funny-walrus`

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `68468ba51131`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.