# Issue

**Title:** relax an interface for input stream

## Description

### Summary

Allow `get_tokens` to accept both file and string input streams.

### Changes

Change its parameter from `std::ifstream&` to `std::istream&` in the declaration and definition. Tokenization behavior is unchanged.

### Validation

All 24 NEP regression cases passed against the pre-PR baseline (`--suite full --program nep`).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `28abdee91805`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.