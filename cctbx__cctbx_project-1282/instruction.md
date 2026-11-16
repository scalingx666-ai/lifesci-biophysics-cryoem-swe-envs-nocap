# Issue

**Title:** Singleton solver when timeout

## Description

## Problem
Reduce2 stopped without performing any optimization on cliques when time ran out with skip_optimizations_after_seconds Phil parameter.

## Changes
Replaced that parameter with treat_cliques_as_singletons_after_seconds and set its default to 300+ years. This will treat all Clique Movers as Singleton Movers when we run past the specified deadline. Setting this to 0 now treats all Cliques as Singletons.

## Tests
Ran on a large-clique file several times with different durations and verified that it switched from Clique to Singleton at the appropriate stage. Verified that it improved the scores of the Cliques that it treated as groups of Singletons.

## Coding agent
Only the autocomplete one that is built into Visual Studio; it selects different ones at different times.

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

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f18206a9bd84`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.