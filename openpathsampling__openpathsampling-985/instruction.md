# Issue

**Title:** Integration tests for SimStore StorableFunctions

## Description

This PR adds some detailed integration tests for storable functions in SimStore. These tests are considerably more complicated than the existing unit tests, and are designed to ensure that storage, backend, and storable functions play together as desired. This also fixes some problems that have previously slipped through the cracks.

A few effects of this:

* Fix issue that `StorableFunction`s didn't show up in `storage.cvs`
* Complete aspects of `MemoryStorageBackend` and `GeneralStorage` that are required to use them (instead of using the SQL backend and the OPS storage)
* Fix various issues with caching results from storable functions (either in storage or in local function cache), especially when dealing with multiple storage files.

EDIT: This also now adds support for a single storable function (CV) to connect to multiple storage backends (e.g., if it was loaded from one file, but it being used to save data into a second file -- want to be able to find disk-cached results in either). If a file is `close`d, then it deregisters itself from all storable functions. If something goes wrong when trying to load from a registered file (e.g., if the user deleted the file in the meantime) a warning will be issued, but we'll fall back to re-evaluating the function if needed. Tests for these behaviors are included in the new test file.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d32c198fc014`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.