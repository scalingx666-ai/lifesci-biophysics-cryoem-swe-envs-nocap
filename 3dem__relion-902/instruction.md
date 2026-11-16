# Issue

**Title:** Add CI based on Github Actions

## Description

It runs on Github-hosted runner natively for Linux x86_64 and in a
emulated (QEMU) Docker container for Linux aarch64

Rlated-to: #896

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `fa923df15ad2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.