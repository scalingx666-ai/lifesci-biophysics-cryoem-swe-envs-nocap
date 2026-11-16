# Issue

**Title:** Allow CPU-only loading of CUDA-packaged models via context manager

## Description

This PR fixes an issue where loading packaged NequIP models on CPU-only systems
fails if the package was created from a CUDA environment.

CPU-only users cannot load some `.nequip.zip` models packaged from CUDA systems
because `torch.package.PackageImporter.load_pickle` may deserialize tensors via
`torch.storage._load_from_bytes`, bypassing `map_location="cpu"`.

This results in the error:
`Attempting to deserialize object on a CUDA device but torch.cuda.is_available() is False`.

To resolve this, a reusable context manager is introduced to ensure CUDA-backed
storages are safely remapped to CPU during deserialization. The fix is applied
locally during package loading, following the context-manager approach suggested
in discussion.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5d84548bfa44`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.