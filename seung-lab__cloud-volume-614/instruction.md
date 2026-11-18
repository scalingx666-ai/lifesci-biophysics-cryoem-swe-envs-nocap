# Issue

**Title:** feat: add skeleton sharded uploads

## Description

Adds logic for doing simple shard uploads for skeletons and adds logic for adding sharding specifications for images, meshes, and skeletons.

The shard specification logic is imported from Igneous.

```
N = 10000000
cv.image.to_sharded()
cv.mesh.to_sharded(num_labels=N)
cv.skeleton.to_sharded(num_labels=N)
cv.commit_info()
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `604b62887a3b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.