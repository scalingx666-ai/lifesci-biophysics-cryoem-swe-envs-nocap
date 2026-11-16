# Issue

**Title:** Release 0.0.81

## Description

## Summary for a scientist

Version bump to **0.0.81**, three changes since 0.0.80:

- **Hyperspectral series analysis** (#654). The hyperspectral agent now analyses a *series* of datacubes (one per temperature, dose, time step, sample) the way the image and curve agents do: the first cube is analysed in full, its verified script is locked and replayed on the others, the series is scouted and split into regimes when the spectra change character, every cube reports the same feature columns, outliers are flagged, failures are re-analysed within a budget, and you get a trend dashboard, series-level claims, an HTML report and a `features.csv` with one row per cube. Replays are cheap (about a minute per cube) and can run in parallel (`series_workers`). In co-pilot / autopilot the regime plan is shown for approval or revision first. Mission control routes a folder of cubes to one such delegation. Anchor code generation now starts from measured facts about the data rather than textbook peak positions.
- **AFM skill: per-grain orientation is gated on resolvability and lattice symmetry** (#649). Orientation is reported only when the lattice is resolvable at the scan's resolution and with the symmetry-correct period; otherwise the skill declines honestly instead of reporting meaningless angles.
- **`fft_defect`: sign-aware deficit gate** (#651, ). Vacancies (dips) get their own optional threshold so they are not under-detected relative to adsorbates (bumps); defaults unchanged.

## Technical

Single change: `pyproject.toml` version `0.0.80 → 0.0.81`. No code changes — the release tag point for merged PRs #649, #651 and #654.

### Release validation (local, mirrors `release.yml`)

- `scripts/build_webui.sh` → bundle built into `scilink/server/static/`.
- `python -m build` → `scilink-0.0.81-py3-none-any.whl` + sdist.
- Wheel carries the React bundle (`server/static/index.html`), `Version: 0.0.81`, and the new `controllers/hyperspectral_series.py`.
- `twine check dist/*` → PASSED (wheel + sdist).
- Offline suite on the merged code: identical failure set to the previous main (environment noise only), 2799 passed.

### After merge

Tag `v0.0.81` on the merge commit and push it — `release.yml` builds the bundled wheel and creates the GitHub release; PyPI upload follows if enabled.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7fded64caec4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.