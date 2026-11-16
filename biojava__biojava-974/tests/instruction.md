# Issue

**Title:** Update ciftools dependency and fix build/tests

## Description

- ciftools 3.0.0 (brings AlphaFold/model support to the table)
- ~propose to ignore `openchart` dependency as it doesn't seem to be used - somebody should confirm this, I've never used any of the forester stuff~
- ~with that exclusion, a dependency on jackson-databind was needed~
- ~`Acetylation_site_dataset.gz` reports 403 in tests and on console, but can be downloaded manually via the browser, added this as 1 MB test resource to circumvent test issues~

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3e83b5b6831e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.