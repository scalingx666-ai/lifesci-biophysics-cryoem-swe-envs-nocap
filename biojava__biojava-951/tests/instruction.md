# Issue

**Title:** CRC64Checksum test #944

## Description

Test for basic behaviour of Crc64Checksum - #944

I think there is a bug exposed by test `partialbyteRange()` and I've proposed a solution in the Crc64Checksum class.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `37991a7bfe41`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.