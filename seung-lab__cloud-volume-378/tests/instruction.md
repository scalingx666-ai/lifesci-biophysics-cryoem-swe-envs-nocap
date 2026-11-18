# Issue

**Title:** perf: skip compression/decompression cycle for caching

## Description

A common use case for caching is to simply copy files in the current compression codec to disk. However, because we were previously unable to acquire files from some providers in the "raw" form, we would have to decompress and recompress everything, which was hugely wasteful. Using the new transcode function in CloudFiles and with (finally) full support for getting raw files, we can skip this step.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `21a26b3cc20e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.