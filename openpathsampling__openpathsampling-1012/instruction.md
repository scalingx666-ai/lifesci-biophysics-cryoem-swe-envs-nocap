# Issue

**Title:** Move MDTrajTopology

## Description

This PR moves `MDTrajTopology` from `openpathsampling.engines.openmm.topology` to `openpathsampling.engines.topology`

This was a slightly bigger hassle than intended (because we want to warn on import). 

I ended up mimicking [`importing ABCs from collections instead of collections.abc is deprecated`](https://github.com/python/cpython/blob/3.7/Lib/collections/__init__.py#L49) 

~This also makes `mdtraj` a hard requirement~

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `cb22b34e29de`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.