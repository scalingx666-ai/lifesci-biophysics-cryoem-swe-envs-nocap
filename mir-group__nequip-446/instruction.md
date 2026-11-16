# Issue

**Title:** Minor Fixes

## Description

This PR includes a few minor cosmetic updates:
- Adds a `CITATION.cff` file to the NequIP repo (Misc project target for `nequip-dev`)
- Updates the `README` and citation page on the docs (previously empty)
- Some cleanup and fixes for the `plot_dimers.py` example script (had some accidental hard-set parameters etc)
- Avoids accidental `""Do NOT manually set PYTORCH_JIT_USE_NNC_NOT_NVFUSER=0..."` warning calls when `PYTORCH_JIT_USE_NNC_NOT_NVFUSER` in `os.environ` (but not set to 0)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d18dd2a6c4c6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.