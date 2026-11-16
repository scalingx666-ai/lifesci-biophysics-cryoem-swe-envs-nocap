# Issue

**Title:** Add `pytest`

## Description

Currently, the builds are published all the time.
This introduces running a quick test, before publishing.

- [ ] should the test also be included into the `.github/workflows/actions.yml` ?
- [ ] bump version to `21.1.4`

The motivation for this PR is, that [v21.1.3](https://github.com/m3g/packmol/releases/tag/v21.1.3) on pypi has some broken wheels whilst [v21.1.2](https://github.com/m3g/packmol/releases/tag/v21.1.2) works, which makes no sense, since there were no real changes between them that could have an effect on that https://github.com/m3g/packmol/compare/v21.1.2...v21.1.3 Thus I assume there was some unexpected behavior in the CI which I can't explain but we can test.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `875ffa431c8e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.