# Issue

**Title:** Upgrade compilers

## Description

Drop unsupported compilers and CentOS 7 container, and add new ones. Now the CentOS 9 is the only Linux platform being tested beside Microsoft/Ubuntu. One of the test cases with GCC 11 is downgraded from C++20 to C++17, to allow more testing there. The cache for the x86_64 Linux job is intentionally invalidated to allow dropping the old compilers' data immediately.

Also fix the Clang static analyzer CI job, which wasn't triggering an error.

Side note: I have also uploaded a CentOS 10 container, but after checking what it comes with it is not sufficiently different to warrant being used yet.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7a5c6a9db936`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.