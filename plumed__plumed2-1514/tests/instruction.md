# Issue

**Title:** configure: do not look for python3 when python is disabled

## Description

.

With `--disable-python`, configure still ran `AC_CHECK_PROGS([PYTHON_BIN],[python3])`, printing `checking for python3...` and then a spurious `cannot enable python interface` warning, even though no python support would be built.

Only search for python3 when the python interface is enabled. The resulting configuration is unchanged: without python support, `PYTHON_BIN` was already reset to empty at the end of the block (so `python_bin` in `Makefile.conf` is empty either way, and the python bindings build in `src/lib/Makefile` is guarded by `ifdef python_bin`).

Changes:
- `configure.ac`: gate the `AC_CHECK_PROGS` call on `test "$python" = true`.
- `configure`: the same edit applied to the generated script (as usual for this repository).

Verification:
- Extracted the generated block and ran it in isolation: with `python=false` the search is skipped and `PYTHON_BIN` stays empty; with `python=true` the search still runs and finds python3 as before.
- `bash -n configure` passes.
- The resulting configuration is identical to the previous behaviour (`PYTHON_BIN` empty when python is disabled).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `952a93131809`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.