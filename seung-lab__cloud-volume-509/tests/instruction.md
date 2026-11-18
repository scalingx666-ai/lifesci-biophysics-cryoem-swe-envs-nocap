# Issue

**Title:** fix: adjustments to get multires meshes displaying correctly

## Description

Hi @SridharJagannathan I was working on implementing multi-res meshes in Igneous. I haven't gotten as far as you or David on producing actual different resolutions, but I did manage to get a single resolution to display. In order to do so, I had to make two changes to CloudVolume that alter the `data_offset` you added to the sharding handler. The changes work, and they seem to conform to the spec, but I wanted to make sure I am not breaking something that is working for you with these changes.

In the Neuroglancer spec, it says that the manifest should be located after the mesh data and implies that the manifest should be the first element accessed. I changed the minishard_index so that the offset points to the beginning of the manifest and the size is the manifest size. 

https://github.com/google/neuroglancer/blob/056a3548abffc3c76c93c7a906f1603ce02b5fa3/src/neuroglancer/datasource/precomputed/meshes.md#sharded-storage-of-multi-resolution-mesh-fragment-data

> If the sharded format is used, the mesh fragment data file is located immediately before the 
> manifest file in the same shard data file. The starting offset within that shard data file is not
> specified explicitly but may be computed from the starting offset of the manifest file and 
> the sum of the mesh fragment sizes specified in the manifest.


I also changed the code that was inactivated in order to make sure the offsets cumulative accounted for the presence of other minishards. 

Let me know if you have any feedback!

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ed42399e7b71`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.