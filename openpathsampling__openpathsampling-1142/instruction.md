# Issue

**Title:** Fix a few more setup -> setup_method tests

## Description

In #1123, most of our setup/teardown methods were fixed to use pytest's preferred (and now required) `setup_method`/`teardown_method` nomenclature. This fixes a few more.

---

**UPDATE**: This fixes 3 problems caused by upstream breaking changes:

1. As mentioned above, pytest now requires `setup_method` / `teardown_method` instead of `setup` and `teardown`.
2. Newer versions of pytest only work with nbval 0.10.0 or later. nbval 0.10.0 introduced what I consider to be a bug, which is that return values that you hide in-notebook with a `;` are seen by nbval as output. This required using `NBVAL_IGNORE` in some notebooks.
3. pytest also broke XUnit-style tests that include staticmethods as tests (I consider this to also be a bug). The fix was to just make them instance methods.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8f1c5afc72b5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.