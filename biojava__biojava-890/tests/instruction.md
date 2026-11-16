# Issue

**Title:** Add GFF3Reader overload that accepts Path

## Description

The String-based functions are still here, and almost all the code is shared.
The Path overload allows everyone to read files via NIO providers, such as e.g.
the GCS NIO provider for reading files on Google Cloud Storage.

Key change: new BufferedReader(new FileReader(filename)) -> Files.newBufferedReader(path)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b9519e4c568e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.