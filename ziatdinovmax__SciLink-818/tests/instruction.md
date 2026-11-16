# Issue

**Title:** ASE is a core dependency

## Description

## What changes for users

A plain `pip install scilink` can now go from analysing a measurement to building an atomic model of it and writing DFT input files. Before, that step stopped with "install scilink[sim]". The web UI's Files tab also draws structure files (POSCAR, `.cif`, `.xyz`) without the extra.

`scilink[sim]` still exists for the heavier simulation pieces: deterministic VASP inputs via atomate2, grain-boundary structures (aimsgb) and molecular-dynamics analysis (MDAnalysis).

Tested live in a fresh Python 3.12 environment with only SciLink and ASE installed (no pymatgen, atomate2, aimsgb, MDAnalysis or mp-api): spectrum-fit-to-DFT-input runs completed end to end with validated VASP inputs, and no missing-module errors. Where a generated structure script reached for pymatgen, it did so for an optional check guarded by `except ImportError`.

---

## Technical details

- **`pyproject.toml`**: `ase>=3.23` moves from `[sim]` to the core dependencies. ASE ships as a wheel and depends only on numpy, scipy, matplotlib and typing_extensions, all already core. `[sim]` keeps `aimsgb`, `atomate2` and `mdanalysis`, with a comment saying what each is for.
- **Streamlit simulate-mode flag (`scilink/ui/_features.py`)**: the probe imported `paramiko` as a sign the user had installed `[sim]`. paramiko became a core dependency with the HPC panel, so the mode was on for every install, including ones without ASE. The probe now imports `ase`, the real requirement. `SCILINK_SIMULATE` still overrides it.
- **React UI**: no visible change. Its server already filters the standalone simulate mode out (`/api/v1/config`), so simulation is reached through the meta agent, which now works on a plain install. The meta's capability cache key includes whether ASE is importable, so an install that lacked it rebuilds that cache once.
- **Messages**: the meta's `delegate_to_simulation` guard and the server's structure-preview error now say `pip install ase` instead of `scilink[sim]`. The guarded imports stay function-local, so the meta still imports if someone removes ASE. One sentence of the meta system prompt now says simulation "needs ASE" instead of "needs the `scilink[sim]` extra".
- **Docs**: `README.md`, `docs/react_web_ui.md` and `CLAUDE.md` updated to match.
- **Tests**: 198 meta, preview, simulation and server tests pass. `tests/test_simulation_orchestrator.py` errors with 14 missing-`model_name`-fixture errors on `main` as well; unrelated.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6fd5875a1af0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.