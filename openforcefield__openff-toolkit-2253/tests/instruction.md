# Issue

**Title:** Replace Mambaforge with Miniforge

## Description

Migrate all references and uses of Mambaforge (now deprecated) to Miniforge (recommended for use)

See

- [x] Tag issue being addressed
- [ ] Add [tests](https://github.com/openforcefield/openff-toolkit/tree/main/openff/toolkit/_tests)
- [ ] Update docstrings/[documentation](https://github.com/openforcefield/openff-toolkit/tree/main/docs), if applicable
- [x] [Lint](https://docs.openforcefield.org/projects/toolkit/en/stable/users/developing.html#style-guide) codebase
- [ ] Update [changelog](https://github.com/openforcefield/openff-toolkit/blob/main/docs/releasehistory.md)

See our [AI Policy](https://github.com/openforcefield/openff-toolkit/blob/main/AI_POLICY.md). Please indicate whether LLMs or other AI-powered tools (beyond simple IDE use cases) were used in this contribution:
- [x] No
- [ ] Yes: [If yes, please provide details here]

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3760dbe9227b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.