# Issue

**Title:** CI: the lowest and the coming versions of NumPy and polars

## Description

NumPy and polars are the two required dependencies, and polars releases often. Until now every CI job installed their newest releases, so the lower bounds in `pyproject.toml` (NumPy 2.3.1, polars 2) were never tested in CI, and a new release was only met at the next change of mdapy.

- **tests.yml**: the Ubuntu job on Python 3.11 now installs the lower bounds of `pyproject.toml` as exact versions (read from the file, through `PIP_CONSTRAINT`), so the oldest Python, NumPy and polars are tested together on every change. The other jobs keep the newest releases. Each job prints the versions it tests.
- **upstream.yml** (new): every Monday and by hand, the full tests on Python 3.14 with the newest releases of NumPy and polars, and with what comes next: the nightly wheels of NumPy and the pre-releases of polars. pytest already turns warnings into errors, so a deprecation of an interface mdapy uses fails here before the interface is removed.
- Contributing and the release notes say so.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `104e15a2618d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.