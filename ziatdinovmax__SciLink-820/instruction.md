# Issue

**Title:** Release 0.0.84

## Description

## Summary

Version bump to 0.0.84. Everything merged to `main` since v0.0.83 (PRs #763 through #819, 150 non-merge commits) ships in this release: native Gatan DM / HyperSpy / EMD-Velox readers (#813), hyperspectral region naming and field overview (#816), the KB folder rule, macOS faiss/OpenMP fix and UTF-8 generated scripts (#817), ASE as a core dependency (#818), the empty-plan guard (#819), Claude Opus 5.5 as the default model with GPT-6 and Gemini 4 options (#808), the web UI HPC MFA panel (#811), and the simulation-side work from #763 onward.

## Validation

Mirrors `release.yml` locally: `bash scripts/build_webui.sh`, `python -m build`, `twine check dist/*` (both PASSED), and `scilink/server/static/index.html` is present in the wheel.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8c72b274af7c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.