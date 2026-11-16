# Issue

**Title:** Startup routine for zndraw

## Description

I worked on a proper zndraw startup routine, so we don't need to hope anymore that the zndraw server and the session is ready. Especially, the sessions often weren't ready, which made the visualizations in the tutorial not reliably display the set camera at all times.

- Using the `/health` endpoint to properly check if the server is up
- Wait for the zndraw session to be ready in the constructor

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6ede307ab775`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.