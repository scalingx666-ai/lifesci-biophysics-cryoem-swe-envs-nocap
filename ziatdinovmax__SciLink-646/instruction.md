# Issue

**Title:** Release 0.0.80

## Description

## Summary for a scientist

Version bump to **0.0.80**, one change since 0.0.79:

- **`read_file` searches no longer pass off a partial result as the document** (#645, ). A search with more than 40 hits now says it is truncated, shows where in the file the rest of the matches are, and can be paged with `match_offset`; the tool descriptions in every mode say the same, so an agent consulting a long PDF narrows or pages instead of citing the front matter.

## Technical

Single change: `pyproject.toml` version `0.0.79 → 0.0.80`. No code changes — the release tag point for merged PR #645.

### Release validation (local, mirrors `release.yml`)

- `scripts/build_webui.sh` → bundle built into `scilink/server/static/`.
- `python -m build` → `scilink-0.0.80-py3-none-any.whl` + sdist.
- Wheel carries the React bundle (`server/static/index.html` + assets), `Version: 0.0.80`, and the search paging in `utils/file_io.py`.
- `twine check dist/*` → PASSED (wheel + sdist).

### After merge

Tag `v0.0.80` on the merge commit and push it — `release.yml` builds the bundled wheel and creates the GitHub release; PyPI upload follows.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7358447d8452`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.