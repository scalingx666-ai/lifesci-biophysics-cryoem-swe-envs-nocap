# Issue

**Title:** SimStore: Fix storable function serialization

## Description

This fixes a bug in SimStore storable functions serialization. When support for function periodicity was added in #990, it looks like I failed to add the periodicity information to the `StorableFunction.to_dict`. This fixes that, and includes a test to prevent regression.

Currently, function periodicity is only used by the (as yet unreleased) command line tools for setting up a simulation, where it is only used at object creation time to determine whether a volume should be periodic or not. This means that the only use case where this bug could be a problem would be if the user is using the unreleased CLI `wizard` to create *new* volumes for periodic CVs *after* loading that CV from a file. This is an unlikely use case, so I don't think this is a critical concern, but it does push for an OPS release before the CLI wizard is released.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ddfe508bbe30`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.