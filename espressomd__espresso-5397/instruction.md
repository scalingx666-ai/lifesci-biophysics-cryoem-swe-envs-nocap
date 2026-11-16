# Issue

**Title:** Add support for C++26

## Description

Description of changes:
- remove array comparison deprecated in C++20 and removed in C++26 ([P2865R6](https://www.open-std.org/jtc1/sc22/wg21/docs/papers/2024/p2865r6.pdf))
- support C++26's new `std::to_string` implementation
- bump Caliper version
- update CI runner images and C++ compiler toolchains and simplify CI/CD workflows

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d8a9164c1933`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.