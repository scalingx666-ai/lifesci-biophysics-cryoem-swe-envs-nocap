# Issue

**Title:** Fix documentation issues

## Description

This PR fixes a few issues in the documentation. Some are typos, others are warnings in Sphinx. I also added a compatibility layer for nbconvert 6, which was previously disallowed by `docs/requirements.txt`. The documentation now builds for nbconvert versions 5 and 6 (the current major version).

- Add engine binary to `.gitignore`.
- Fix warning in Sphinx from citation names.
- Use `template_name` instead of `template_file` for compatibility with nbconvert 6 (also tested to work with version 5.6.0).
- Require nbconvert>=5.

P.S. I'm planning to submit some more substantive pull requests soon. I just wanted to separate out my documentation fixes from the feature work I'm doing for #1055.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5035ba5784c5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.