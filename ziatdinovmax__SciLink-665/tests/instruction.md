# Issue

**Title:** Expose __version__ on the scilink package

## Description

## Summary
- Adds `__version__` to `scilink/__init__.py` using `importlib.metadata` to read the version from `pyproject.toml` at import time
- Falls back to `"0.0.0+unknown"` when the package is not installed (e.g. a bare checkout without `pip install -e .`)
- Adds `__version__` to `__all__`

## Motivation
`scilink.__version__` is the standard way Python packages expose their version, but it wasn't available — `python -c "import scilink; print(scilink.__version__)"` raised `AttributeError`. This caused issues during benchmarking on HPC where we needed to verify which version was installed across multiple conda environments.

## Test plan
- [x] `python -c "from scilink import __version__; print(__version__)"` prints the installed version
- [x] Uninstalled checkout falls back to `"0.0.0+unknown"` without error

🤖 Generated with [Claude Code](https://claude.com/claude-code)

https://claude.ai/code/session_01XYvkT6x9VFTBCyY33DMLPL

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `16574b66c9a4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.