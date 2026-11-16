# Issue

**Title:** docs: add AGENTS.md and PR template for coding agents

## Description

- Add an AGENTS.md and PR template to help coding agents.
- Specific examples added to CONTRIBUTING.md and general clean up

The plan is to merge on Monday. Before the merge, I will add a `CLAUDE.md` file that only contains `@AGENTS.md`. This will conflict or clobber any personal `CLAUDE.md` you may already have. The reason is that `claude` will ignore `AGENTS.md` if `CLAUDE.md` exists. Any personal rules should be moved to your global (or other scope) `CLAUDE.md` before Monday.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `bee33903a5cc`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.