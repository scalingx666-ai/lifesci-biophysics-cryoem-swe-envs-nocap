# Issue

**Title:** feat(image): download an image already renumbered

## Description

This can save 2-4x on memory usage.

```python
lbls, remap = cv.download(cv.bounds, renumber=True)
```
If you download a large volume as a uint64 but it could be represented using a uint16, you can theoretically resize it, but that requires allocating the larger memory structure, allocating more, and then downsizing it. With this approach, you never allocate the larger data type and render the volume in renumbered form.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d84b9e6ed76e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.