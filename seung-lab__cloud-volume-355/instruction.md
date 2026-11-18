# Issue

**Title:** adding smart graphene multi-level mesh stitching

## Description

I have implemented a more elaborate mesh stitching routine for graphene sharded.  It now takes into account the mesh level of each fragment that is downloaded, in order to label the chunk boundaries dynamically on each fragment in a way that scales with the level of the chunk the fragment came from, and then only removes duplicate vertices on those chunk boundaries.

To do so, i did a little refactoring of the deduplication code so i could call it outside of the Mesh class.  Happy to consider refactoring this in some way, and might be duplicating effort that exists elsewhere in pychunkedgraph. 

I've tested that this enables proper mesh stitching of graphene sharded meshes on a proofread ID example from minnie, but haven't tested it on fly.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `da9ba287001f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.