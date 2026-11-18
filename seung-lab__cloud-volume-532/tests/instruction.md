# Issue

**Title:** feat: cv.image.download_files

## Description

Download image files without rendering into a numpy array.

```
a_dict = cv.image.download_files(bbox, mip, decompress=True)
```

This could be useful for creating larger shard files in Igneous as it helps reduce memory pressure substantially.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b802385dcb3a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.