# Issue

**Title:** perf(server): serve requests concurrently

## Description

Using `ThreadingHTTPServer` allows multiple requests at a single time and prevents a singel slow download from blocking other requests. Because each request gets its own handler and CloudFiles instance, the states should nto be shared.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a8e5e6c9f9bf`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.