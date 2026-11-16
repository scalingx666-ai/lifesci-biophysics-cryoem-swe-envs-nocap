# Issue

**Title:** Fix np.object AttributeError in PeriodicKDTree.query_ball_point

## Description

## Summary

`mbuild/periodic_kdtree.py` uses the removed alias `np.object`:

```python
result = np.empty(retshape, dtype=np.object)
```

`np.object` was deprecated in NumPy 1.20 and **removed in NumPy 1.24**. Since `environment.yml` / `environment-dev.yml` pin `numpy>=2.0,<2.6`, this alias no longer exists in any supported NumPy, so the line raises `AttributeError: module 'numpy' has no attribute 'object'`.

This path is hit whenever `PeriodicKDTree.query_ball_point()` is called with an **array of multiple points** (the `len(x.shape) != 1` branch), so the multi-point query is currently broken.

## Fix

Replace `np.object` with the builtin `object` — the exact [replacement recommended by NumPy](https://numpy.org/devdocs/release/1.20.0-notes.html#deprecations). Behavior is identical (an object-dtype array), and it works across all supported NumPy versions.

```diff
-            result = np.empty(retshape, dtype=np.object)
+            result = np.empty(retshape, dtype=object)
```

One-line, behavior-preserving.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3e4bcf11c39e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.