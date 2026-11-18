# Issue

**Title:** feat: read skeleton spatial index + sharded format writer

## Description

Enable reading the new JSON spatial index on skeletons in order to query skeletons by bbox.

Igneous can now produce a regular grid of JSON files, with names specified as the physical bounding box + ".spatial" (e.g. 0-100_0-100_0-100.spatial) which consists of all skeletons contained therein with their sub-bounding box (so more precise queries are possible). 

The spatial index query feature looks like this:

```python
skels = cv.skeletons.get_by_bbox( Bbox( (10, 10, 10), (30, 30, 30) ) )
skels = cv.skeletons.get_by_bbox( np.s_[ ... ] )
```
It works by downloading all spatial indices intersecting the specified bounding box, eliminating the labels that do not intersect on their sub-bounding box, and then downloading the survivors.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d40886567d32`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.