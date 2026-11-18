# Issue

**Title:** perf: use simplejpeg for JPEG encoding/decoding

## Description

simplejpeg uses an updated version of libjpeg-turbo which can be much faster than the version of libjpeg used by Pillow.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `da41ebd15d35`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.