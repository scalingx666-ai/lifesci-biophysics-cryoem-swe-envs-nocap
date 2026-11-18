# Issue

**Title:** feat: download sharded meshes based on mesh manifests

## Description

Enables `cv.mesh.get` for Graphene sharded meshes. There are two special parameters this PR adds to the `get` function as well.

Ordinarily for Graphene, the `cv.mesh.get` function will contact the PCG server to request a manifest. Since the server, in order to traverse the PyChunkGraph octree to find the child labels with actual meshes, performs a series of `exists` calls, it already does the work of finding the index and minishard index of each shard. It therefore returns the manifest in an atypical fashion: 
```json
{
  "fragments": [
       "~3/47461891-0.shard:3430219:1073", 
       "~3/45385220-0.shard:310763:1182", 
       "~3/42020357-0.shard:2270331:304"
   ],
}
```
Here the tilde means the mesh is an initial sharded mesh, the 3 means layer 3 of the PCG, she middle section is the name of the shard file, and the two numbers separated by colons are the byte offset and the size of the data in bytes.  

Using this data allows us to rapidly retrieve the mesh. However, this severely breaks encapsulation of the shard accessing logic and so may be non-functional in exceptional circumstances. For example, such as when developing a second mesh with different shard parameters while maintaining the older mesh for proofreading and visualization concurrently. Since the new mesh filenames and byte offsets would be different, this model becomes unworkable. 

Therefore, we have to additional modes: `use_byte_offsets` and `bypass` modes. 

`cv.mesh.get(..., bypass=True)` mode bypasses the PCG server and fetches meshes directly from the storage media based solely on their segment ID. It first checks for a dynamic version of the mesh, and if that fails, tries from the initial shards.  

`use_byte_offsets=False` mode is a hybrid mode between normal and `bypass` modes. We request a manifest from PCG, but ask it to return mesh segment IDs. We then use these labels in bypass mode to construct the mesh. This would allow us to access a mesh under the conditions described above.

```
/manifest/864691135492962144:0?return_seg_ids=1

 "seg_ids": [
    241653690820204416, 
    240538786566506912, 
    238732289498941469, 
    241256767659446532, 
```

Unfortunately, the implementation of bypass mode is currently very slow.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1284a30f55b9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.