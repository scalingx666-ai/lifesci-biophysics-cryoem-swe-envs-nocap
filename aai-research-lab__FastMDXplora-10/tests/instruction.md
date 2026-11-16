# Issue

**Title:** Adaptive plotting helpers for slide-ready figures

## Description

introduce auto_ticks/apply_slide_style plus unit tests
refactor all analysis plotters to use the shared styling helpers
re-export plotting utilities from fastmdanalysis.utils

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ee5d35a1e7a3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.