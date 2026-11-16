# Issue

**Title:** fix(validation): make ligand contact comparison trustworthy

## Description

## Summary
- forward explicit ligand residue names through the analysis CLI
- align the ProLIF hydrophobic cutoff with the native 4.0 A rule
- exclude native interaction families explicitly recorded as not measured
- reimage ligands into the protein minimum-image copy before ProLIF
- support ligand-free negative controls without fabricating an interaction table
- ignore local `.hermes/` validation artifacts

## Tests
- `env -u PYTHONPATH -u PYTHONHOME .venv/bin/pytest -q` — 3040 passed, 82 skipped, 105 deselected
- affected clean-branch suite — 139 passed, 1 skipped

## Scope
This PR contains code/tests only. Validation reports, run outputs, and local configuration remain untracked and ignored under `.hermes/`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `14d279650ff1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.