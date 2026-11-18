# Issue

**Title:** Delay creating metadata in dask.to_cloudvolume

## Description

I was finding a huge slowdown when opening many volumes for writing on a cloud filesystem.  All the metadata files were being written from my machine -- instead of being sharded to workers.  

In addition, when compute=False, it may not be the intention of the caller to ever write out data.  The PR delays the writing of `info` or `provenance` until compute time.

For these reasons, I think we can delay writing the metadata.  It's an open question to me if we should always wrap in `dask.delayed`, regardless of the value of compute.  I believe this may be better, and am trying to open a discussion with the dask maintainers on a similar PR in their repo - dask/dask#5797

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8a1539b36a96`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.