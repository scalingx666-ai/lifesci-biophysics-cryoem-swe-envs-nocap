# Issue

**Title:** mmtbx.development.naiad: rename to mmtbx.development.virvel

## Description

## Problem
naiad was a working name.

## Changes
- virvel is Swedish for whirl or eddy.
- Interface change: `mmtbx.development.naiad` is removed, with no alias.
  The `water_protonation` modules keep their names.

## Tests
No tests added: only the command name and docstrings change.
`mmtbx.development.virvel` runs (macOS); mmtbx suite (Linux): no new
failures.

## Coding agent

| Field | Value |
|-------|-------|
| Model and version | Claude Opus 5.5 (claude-opus-5-5) |
| Harness and version | Claude Code 2.1.311, macOS |

## Checklist
- [x] Searched open and closed PRs and issues for prior work (#1175 and
      #1285 added the program; no earlier rename)
- [x] New or changed tests are registered in the module's `run_tests.py`
- [x] Docstrings and PHIL `.help` strings cover the new or changed behavior
- [x] No new external dependency, or it was agreed with the developers
- [x] No imports of modules outside this repository; `chem_data` and
      `phenix_regression` are used only for data, with a skip when absent
- [x] Interface or breaking changes are called out under Changes
- [x] A human has reviewed the complete diff

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4ea8b7bbfc3c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.