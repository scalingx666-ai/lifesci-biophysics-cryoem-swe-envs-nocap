# Issue

**Title:** Respect compression settings for skeleton uploads.

## Description

A quick fix to respect compression settings when uploading skeletons.

I do replicate this logic from `get()`:

```python
    compress = self.config.compress 
    if compress is None:
      compress = True
```

Is this logic encapsulated somewhere else in cloud-volume? It seems like it should probably not be replicated. (Perhaps this could be done by making config.compress a property?)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4317c609372f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.