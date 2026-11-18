# Issue

**Title:** Support for neuroglancer multi-resolution sharded meshes

## Description

This is an initial effort to read [neuroglancer](https://github.com/google/neuroglancer/)'s [multi-resolution sharded mesh format](https://github.com/google/neuroglancer/blob/master/src/neuroglancer/datasource/precomputed/meshes.md).

**TODO **

- [x] Request specific levels of detail
  - [x] ... and only request the relevant fragments over http
- [X]  [Convert](https://github.com/google/neuroglancer/blob/master/src/neuroglancer/datasource/precomputed/meshes.md#multi-resolution-mesh-fragment-data-file-format) from "stored model" space coordinates into correct model coordinates
- [X] Convert from "model space" into physical space
- [X] Concatenate meshes into a single mesh
- [X] Match function of other cloud-volume mesh functions
- [~] Check for missing manifests
  - (Via bulk prefetching of all manifests?)
  - [X] Added an exists method
- [ ] General code cleanup...

** Other issues (that probably won't be addressed) **
- read-only
  - Eventual write support would be done via unsharded storage & a compaction stage
- Only scale parameters in the homogenous transform are supported
-  Support for unsharded storage of multi-resolution meshes (much of the code would be similar, e.g. the mesh fragments are the same)
- Fuse meshes? (not sure how important this is)

Sample code
```python
import cloudvolume
test_ids = [ 710435991, 8392766771, 8393399736 ]
vol = cloudvolume.CloudVolume('https://storage.googleapis.com/fafb-ffn1-20190805/segmentation', mip=0, cache=False)
# Returns {'seg': Mesh}
for lod in reversed(range(4)):
   meshes = vol.mesh.get(test_ids, lod=lod, concat=True)
   for segment in meshes:
       mesh = meshes[segment]
       mesh.viewer()
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `47c4653dc482`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.