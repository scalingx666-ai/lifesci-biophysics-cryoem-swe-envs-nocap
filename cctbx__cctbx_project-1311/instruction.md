# Issue

**Title:** CI: stop setting PYTHONTRACEMALLOC in the quick workflow

## Description

## Problem
The quick workflow runs the tests with `PYTHONTRACEMALLOC=1`, which
records a traceback for every memory allocation. It makes the CI tests
2.7 to 4 times slower overall, and tests that loop in Python up to 25
times slower.
The only thing it adds is "Object allocated at" lines under
`ResourceWarning`s.

## Changes
- Remove `PYTHONTRACEMALLOC=1` from the unix and Windows test steps.
  `PYTHONDEVMODE=1` stays, so warnings are still printed and still fail
  `raise_if_errors()`.
- Drop a timing note in `tst_mmcif_hierarchy_parity.py` that quoted the
  variable.

## Tests
Run 43525849174 on this branch, against run 43506004119 (the same
workflow, with the variable):

| | ubuntu | macOS | Windows |
|---|---|---|---|
| "Run tests" step | 18m23s → 7m29s | 33m17s → 10m19s | 49m33s → 18m54s |
| Whole job | 23m04s → 11m45s | 40m03s → 16m03s | 63m42s → 33m35s |
| Sum of test times | 4114 → 1379 s | 7155 → 1778 s | 11610 → 4388 s |
| `tst_two_atoms.py` | 414 → 27 s | 549 → 22 s | 617 → 62 s |
| `tst_atom_curves.py` | 304 → 41 s | 411 → 29 s | 510 → 91 s |

Every one of the 698 tests in both runs ends with the same status. Tests
with stderr output: the same count on ubuntu and macOS, 30 → 29 on
Windows.

## Coding agent

| Field | Value |
|-------|-------|
| Model and version | Claude Opus 5.5 (claude-opus-5-5) |
| Harness | Claude Code (VS Code extension) |
| Rounds of adversarial review | None; the person directing judged a 3-line CI change did not need one |
| Reviewer model, version, and harness | Not applicable |

## Checklist
- [x] Searched open and closed PRs and issues for prior work (none found)
- [ ] New or changed tests are registered in the module's `run_tests.py`
      (not applicable)
- [ ] Docstrings and PHIL `.help` strings cover the new or changed
      behavior (not applicable)
- [x] No new external dependency, or it was agreed with the developers
- [x] No imports of modules outside this repository; `chem_data` and
      `phenix_regression` are used only for data, with a skip when absent
- [x] Interface or breaking changes are called out under Changes
- [x] Agent-written code: the writing agents had `AGENTS.md` and
      `CONTRIBUTING.md` in context, and the reviewing agents had
      `AGENTS.md`
- [x] A human has reviewed the complete diff

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `60baeb4e9ccc`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.