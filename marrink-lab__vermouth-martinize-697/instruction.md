# Issue

**Title:** Idr annotation fix

## Description

Currently the `-id-regions` flag only accepts resid regions, with no chain specifier. This PR fixes that by slightly changing the input to be more like the modifications style, `<chain>-<resid_start>:<resid_end>`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `792cefd316b7`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.