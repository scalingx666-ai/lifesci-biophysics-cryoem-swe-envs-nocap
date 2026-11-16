# Issue

**Title:** Remove Documenter from the test dependencies

## Description

Documenter isn't used by the tests: it's only needed in the docs environment (`docs/Project.toml`), which keeps it. This removes it from `[extras]`, the test target and `[compat]`.

Julia 1.10 support is unchanged. This replaces the Documenter part of #198, which is closed, since CellListMap keeps supporting Julia 1.10.

Tested locally: all 2,458 tests pass.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ec1f6fba5ed7`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.