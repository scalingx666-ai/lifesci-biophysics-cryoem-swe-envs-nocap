# Issue

**Title:** Separate NAMD GlobalMaster and colvarproxy_namd classes

## Description

Currently, `colvarproxy_namd` (also known as `GlobalMasterColvars`) is a derived class of `GlobalMaster`, consistent with what other NAMD features based on global forces do. This inheritance worked okay as long as we were using `GlobalMaster` as the API for communicating with NAMD. However, this also presented significant issues in #783, which added support for `CudaGlobalMasterClient`. The most significant was the duplication of functionality that is native to `colvarproxy_namd`, but could not be used without enabling the traditional `GlobalMaster` class as well.

This PR removes the inheritance between `GlobalMaster` and `colvarproxy_namd`, adding a new `GlobalMasterColvars`, which wraps around `GlobalMaster`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e49fa5bc5765`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.