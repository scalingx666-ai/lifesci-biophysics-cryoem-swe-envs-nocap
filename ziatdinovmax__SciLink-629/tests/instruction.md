# Issue

**Title:** Release 0.0.77

## Description

## Summary for a scientist

Version bump to **0.0.77**, rolling up everything merged since 0.0.76. Highlights:

- The **React web UI is now the default** for `scilink ui` (`--streamlit`
  still available); the web backend is a core dependency, so a plain
  `pip install scilink` gets it.
- **Embeddings** default to keyword-only (no model/key needed); Bedrock
  auto-selects Titan; an embeddings-only base URL is supported.
- Cross-provider **tool-schema portability** and internal-proxy support (#606).
- **TLS** for the web server (#611), the **derive-from-outputs** tool (#599),
  and UI polish + background-output routing fixes.

## Technical

Single change: `pyproject.toml` version `0.0.76 → 0.0.77`. No code changes —
this is the release tag point for merged PRs #605–#628.

### Release validation (local, mirrors `release.yml`)

- `scripts/build_webui.sh` → bundle built into `scilink/server/static/`.
- `python -m build` → `scilink-0.0.77-py3-none-any.whl` + sdist.
- Wheel carries the React bundle (`server/static/index.html` + 63 assets),
  `Version: 0.0.77`, `Requires-Dist: fastapi>=0.110` (web deps now core).
- `twine check dist/*` → **PASSED** (wheel + sdist).

### After merge

Tag `v0.0.77` on the merge commit and push it — `release.yml` builds the
bundled wheel and creates the GitHub release. PyPI upload is manual (or via
trusted publishing if enabled).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4a88d0e71f94`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.