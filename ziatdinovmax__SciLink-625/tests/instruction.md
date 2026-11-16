# Issue

**Title:** Make the React web UI the default for `scilink ui`

## Description

## Summary for a scientist

Running `scilink ui` now opens the new **React web UI** — the same modern
interface as `scilink-web`, with live streaming, a file explorer, a
telemetry view for mission-control sessions, and remote-serving support —
instead of the older Streamlit app. Nothing changes about how you install
SciLink: `pip install scilink` already ships the web UI, and your existing
sessions open the same way (the two UIs share session folders and
checkpoints).

If you prefer the classic Streamlit interface, or you're on a source
checkout that hasn't built the React bundle, use:

```bash
scilink ui --streamlit
```

You don't have to remember that on a fresh git checkout: if the React
bundle isn't built, `scilink ui` automatically falls back to Streamlit and
prints a one-line note. Simulate-mode sessions, which the React UI doesn't
cover yet, are also reached through `--streamlit`.

## Technical

- **`scilink/cli/ui.py`** rewritten: `main()` launches the React web UI by
  default via `scilink.server.cli.main(argv)`. `--streamlit` (stripped from
  argv) forces the classic app through the extracted `_run_streamlit()`.
  `_react_bundle_present()` checks `webui/dist/index.html` or
  `server/static/index.html` (mirroring `server/app.py`); when absent,
  `main()` falls back to Streamlit with a stderr note. This keeps the flip
  safe: source/git installs without Node, and the simulate-mode gap, are
  both covered by the Streamlit path.
- **`pyproject.toml`**: `fastapi>=0.110`, `uvicorn[standard]>=0.29`,
  `python-multipart>=0.0.9` move from the `[web]` extra into core
  `dependencies`; `web = []` kept as an empty back-compat alias so
  `pip install scilink[web]` still resolves. Release wheels already carry
  the built bundle (built by `scripts/build_webui.sh` in `release.yml`).
- **`scilink/cli/main.py`**: `ui` usage text updated to reflect the React
  default and the `--streamlit` escape hatch.
- **Docs**: README, `docs/cli_reference.md` (table row + a new
  `scilink ui` subsection), `docs/react_web_ui.md` (intro, quick start,
  fallback note), and the `docs/react_ui_roadmap.md` standing decision
  (records that the flip shipped 2026-09-13 with Streamlit kept as a
  fallback rather than retired).
- **Tests**: `tests/test_ui_react_default.py` pins the dispatch — React
  when the bundle is present, Streamlit under `--streamlit`, Streamlit
  fallback (with a note) when no bundle — plus the pyproject dependency
  layout (web deps in core, `[web]` empty).

### Verification

- New dispatch test: 4/4 pass.
- UI + web-server unit suite: 108 passed; the only failures
  (`test_ui_reattach_apptest.py`, ×3) are pre-existing in the baseline
  (a CWD-dependent `AppTest.from_file` path issue), not regressions.
- Live smoke: `scilink ui` serves the built React bundle at `/`
  (`index.html` with hashed assets) and `/api/v1/config` returns 200;
  `scilink ui --streamlit` boots the Streamlit app (`/_stcore/health` → ok).

Streamlit is intentionally **not** removed — it remains the documented
`--streamlit` fallback and the server keeps reusing its streamlit-free
modules.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `26df9bcc3467`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.