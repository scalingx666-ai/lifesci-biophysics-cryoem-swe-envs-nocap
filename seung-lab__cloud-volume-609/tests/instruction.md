# Issue

**Title:** feat: shard->shard transfers, memory cutout, transcoding images

## Description

Adds:
- unsharded->unsharded transcoding (transfer_to pre-existed)
- shard->shard + transcoding
- any -> unsharded + transcoding
- Anonymous memory based cutouts via cv.image.memory_cutout

Not covered:
- unsharded -> sharded (but it is in Igneous), it probably wouldn't be too much work but requires importing logic from igneous

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b05478de3426`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.