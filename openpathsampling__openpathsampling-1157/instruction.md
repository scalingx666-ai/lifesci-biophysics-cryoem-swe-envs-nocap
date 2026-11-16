# Issue

**Title:** SimStore performance improvements and fixes

## Description

This includes several performance improvements and minor fixes for SimStore. Quick manifest:

* Added and started using `is_storage_string` and `has_storage_uuid`, which use the fast `ClassIsSomething` model to cache classes with given attributes. The normal `is_string` and `has_uuid` methods were taking a larger fraction of time than they should.
* Added default support for SQL index on UUID (which should have been there all along).
* Fix an edge case where old-style CVs were no reloaded correctly in SimStore, because the new JSON serialization was already making a callable and the code to make a callable from the marshal data was no longer needed.

This also helps clear up some of the external testing in ops-storage-notebooks which has been off for a while.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `bbfe5d799210`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.