# Issue

**Title:** fix pytest / `build[uv]`

## Description

This PR fixes
- `build-frontend = "build[uv]"` not available by default on mac / windows runners, so reverting to default
- fix pytest with `/` and `\` issue on windows
- allow workflow dispatch to publish

I released https://pypi.org/project/packmol/21.1.4/ so no new release would be required.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b6f29792dc90`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.