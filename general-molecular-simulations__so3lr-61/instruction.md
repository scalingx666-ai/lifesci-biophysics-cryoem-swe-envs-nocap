# Issue

**Title:** README: SO3LR Tauri banner as the logo

## Description

Replaces the README logo with the SO3LR Tauri banner: the wordmark, the tagline and four biomolecules (protein, DNA, lipid bilayer, glycoprotein).

- `so3lr/logo.png` keeps its path, so the packaging rules in `pyproject.toml` are unchanged.
- The image has a transparent background and reads on both GitHub themes.
- The README shows it centred at 750 px; on narrow screens it fills the width.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d235d6d60430`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.