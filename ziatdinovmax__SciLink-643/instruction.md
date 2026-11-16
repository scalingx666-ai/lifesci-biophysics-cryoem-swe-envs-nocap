# Issue

**Title:** Release 0.0.79

## Description

## Summary for a scientist

Version bump to **0.0.79**, rolling up what merged since 0.0.78:

- **Dollar signs render as currency, not math** (#640, ): "~$0.79/m³ … $300/t" no longer turns into italic formulas or strikethrough in chat answers, previews and report cards; `$$ … $$` formulas still render.
- **A document carries its workflow figure once** (#642, ): the workflow diagram is appended only when the author has not already embedded one.
- **Feedback-driven refinements are labelled as such** (#641, ) in the log, matching the report.
- **AFM skill and image code generation**: color-to-value legends and robust per-row leveling (#637).
- **Simulation software availability** is re-probed on cache load so new installs are seen, and the UMA backend deploys (#607).

## Technical

Single change: `pyproject.toml` version `0.0.78 → 0.0.79`. No code changes — this is the release tag point for merged PRs #607, #637, #640, #641 and #642.

### Release validation (local, mirrors `release.yml`)

- `scripts/build_webui.sh` → bundle built into `scilink/server/static/`.
- `python -m build` → `scilink-0.0.79-py3-none-any.whl` + sdist.
- Wheel carries the React bundle (`server/static/index.html` + assets) with the single-dollar math switch compiled in, `Version: 0.0.79`, and the workflow-figure check in `orchestrator_tools.py`.
- `twine check dist/*` → PASSED (wheel + sdist).

### After merge

Tag `v0.0.79` on the merge commit and push it — `release.yml` builds the bundled wheel and creates the GitHub release; PyPI upload follows manually.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `67ee27121365`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.