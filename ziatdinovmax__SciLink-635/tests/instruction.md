# Issue

**Title:** Release 0.0.78

## Description

## Summary for a scientist

Version bump to **0.0.78**, rolling up the three fixes merged since 0.0.77:

- **Knowledge bases remember which embedding model built them** (#632, ). Attaching a KB to a session that uses a different embedder now says so once and searches by keyword, instead of silently failing every query with an empty "Dense KB retrieval failed ()". Older KBs get a clear dimension message on first use.
- **The TEA's evidence record no longer shows up as a report card** (#634, ). The retrieved-chunk file the TEA keeps for its critic stays out of the answer in both the React and Streamlit UIs; your actual reports embed as before.
- **Start Session cannot be double-clicked** (#630). The sidebar locks while a session is being created.

## Technical

Single change: `pyproject.toml` version `0.0.77 → 0.0.78`. No code changes — this is the release tag point for merged PRs #630, #632 and #634.

### Release validation (local, mirrors `release.yml`)

- `scripts/build_webui.sh` → bundle built into `scilink/server/static/`.
- `python -m build` → `scilink-0.0.78-py3-none-any.whl` + sdist.
- Wheel carries the React bundle (`server/static/index.html` + 63 assets), `Version: 0.0.78`, `Requires-Dist: fastapi>=0.110`, and the new code paths (`_index_meta_path` in `knowledge_base.py`, `_EVIDENCE_SUFFIXES` in `server/artifacts.py`).
- `twine check dist/*` → PASSED (wheel + sdist).

### After merge

Tag `v0.0.78` on the merge commit and push it — `release.yml` builds the bundled wheel and creates the GitHub release. PyPI upload stays manual unless `PYPI_PUBLISH` is set.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `80bda6298e71`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.