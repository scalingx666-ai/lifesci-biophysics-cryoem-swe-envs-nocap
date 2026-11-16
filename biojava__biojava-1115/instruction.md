# Issue

**Title:** Switch to new maven central plugin

## Description

OSSRH reached end of life on June 30, 2025.

This uses the new maven central plugin as per recommendations : https://central.sonatype.org/pages/ossrh-eol/#publishing-to-central-portal

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5a699eb64655`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.