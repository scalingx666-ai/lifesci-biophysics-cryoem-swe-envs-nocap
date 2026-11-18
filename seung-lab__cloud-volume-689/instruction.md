# Issue

**Title:** perf: Store numpy arrays directly in LRU cache instead of re-encoding

## Description

When lru_encoding is "raw", store decoded numpy arrays directly in the LRU cache instead of re-serializing them via encode_raw (which does np.asfortranarray + tobytes). This avoids significant overhead when reading non-"raw" data.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `eaa6a5ad28b2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.