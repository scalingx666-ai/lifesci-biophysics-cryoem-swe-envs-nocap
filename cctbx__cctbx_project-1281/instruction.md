# Issue

**Title:** Sort cliques

## Description

<!--
Reduce2 now sorts cliques from smallest to largest before solving them.
-->

## Problem
When running with a time-limited solver, this completes as many cliques as possible before giving up.

## Changes
Reduce2 now sorts cliques from smallest to largest before solving them.

## Tests
Gets the same results. I verified by looking at the output of a time-limited run: `python cctbx_phenix\modules\cctbx_project\mmtbx\regression\tst_reduce.py` and making sure it solved them in the proper order.

## Coding agent
None

## Checklist
- [x] Searched open and closed PRs and issues for prior work (link them,
      or say "none found")
- [x] New or changed tests are registered in the module's `run_tests.py`
- [x] Docstrings and PHIL `.help` strings cover the new or changed behavior
- [x] No new external dependency, or it was agreed with the developers
- [x] No imports of modules outside this repository; `chem_data` and
      `phenix_regression` are used only for data, with a skip when absent
- [x] Interface or breaking changes are called out under Changes
- [x] A human has reviewed the complete diff

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e1d7b005974c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.