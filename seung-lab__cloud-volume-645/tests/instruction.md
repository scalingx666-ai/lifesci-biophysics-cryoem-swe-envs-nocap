# Issue

**Title:** feat+perf: allow specifying LRU encoding type

## Description

This PR adds the `lru_encoding` flag to CloudVolume at the top level. This allows you to choose the image encoding type of the LRU. This can be useful for either conserving additional space by choosing a higher compression type or for accelerating chunk reuse by selecting e.g. `raw` at the expense of more memory.

There may even be times when you can pick a faster and smaller compression type for certain queries.

The default value for `lru_encoding` is `"same"` which is whatever the current mip level's encoding is.

This change also makes parallel download compatible with non-raw LRU at the expense of additional encoding time.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `bb214e7f17fc`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.