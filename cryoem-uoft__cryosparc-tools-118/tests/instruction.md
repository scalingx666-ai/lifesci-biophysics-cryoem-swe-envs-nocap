# Issue

**Title:** fix(spm): various bugs in new API integration

## Description

Introduced in #114 

- Restore uploading text file handles to projects (useful for .star files)
- Add "Stability" column to printed job specs
- Add "Passthrough?" column to printed output specs
- Documentation fixes

We should still run the notebooks against a full instance once the full integration is complete and the new API is running on our internal `develop` instance.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4cb88d864004`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.