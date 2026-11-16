# Issue

**Title:** Bump the development version to 7.3.0-SNAPSHOT

## Description

The electron density work and the FileDownloadUtils changes that go with it add public API and change a default, which belongs in a minor release rather than a patch. Their @since tags say 7.3.0, so the project version should agree.

Standalone so it can be taken or dropped independently of the feature work, and so the release manager decides when it lands.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2a57e199c763`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.