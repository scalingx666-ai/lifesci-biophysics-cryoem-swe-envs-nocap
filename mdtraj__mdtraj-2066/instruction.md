# Issue

**Title:** Drop numpy1 from testing/support, not building freethreading wheels, build linux-arm64 wheels

## Description

Staying in line with SPEC 0, dropping numpy1 testing and support. Should speed up CI too, due to the need for fewer macos runners.

Also blocked freethreading wheels from building, while I'm at it.  for more information.

Also building wheels for linux-arm64. We're testing it already, why not build them?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ed85118b00f9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.