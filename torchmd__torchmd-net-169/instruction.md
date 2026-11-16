# Issue

**Title:** Adding a cell list neighbor list module

## Description

This PR includes a new module called DistanceCellList (looking for a better name), which is an alternative to the [Distance](https://github.com/torchmd/torchmd-net/blob/main/torchmdnet/models/utils.py#L195-L203) module that provides three strategies to find neighbors:
 1. The O(N^2) [getNeighborPairs functionality from NNPops](https://github.com/openmm/NNPOps/blob/master/src/pytorch/neighbors/getNeighborPairs.py), referred to as **brute**, which required making it batch-aware. The batching functionality is actually a really minimal modification, so this could be upstreamed to NNPops and that used instead.
 2. A **cell** list distance module, using a classic spatial hashing approach (see [here](https://uammd.readthedocs.io/en/latest/NeighbourList.html#celllist))
 3. An improved O(N^2) approach referred to as **shared** (from NVIDIA [here](https://developer.nvidia.com/gpugems/gpugems3/part-v-physics-simulation/chapter-31-fast-n-body-simulation-cuda))

The current Distance module has some drawbacks:
 1. Really slow for a single batch
 2. Does not understand periodic boundary conditions
 3. Incompatible with CUDA graphs
 4. Always returns redundant pairs (give you i,j and j,i). This is required for the current Message Passing modules.
 
The new module solves all these by being:
1. Two orders of magnitude faster for a single batch (while being at least as fast as Distance in all instances tested)
2. CUDA graph compatible (and jit.script compatible)
3. Drop in replacement to Distance when using default parameters*.
4. Compatible with periodic boundary conditions.
5. Able to optionally skip redundant neighbors, saving time and memory.
6. 
*There is a caveat, Distance requires max_num_neighs (maximum allows neighbors per particle), DistanceCellList requires max_num_pairs (maximum number of total neighbor pairs).
```python
#From DistanceCellList:
"""
        max_num_pairs : int
            Maximum number of pairs to store.
            If negative, it is interpreted as (minus) the maximum number of neighbors per atom.
"""
```
This is the current declaration of the new module:
https://github.com/torchmd/torchmd-net/blob/730b0a17c6dc4d4fdc2b1d9b8769c80cfd765210/torchmdnet/models/utils.py#L81-L129

### Changes to the installation process

This operation is written in C++ and CUDA. TorchMD-Net does not currently have an in place build system being only python.
I build this as a [torch cpp_extension with jit compilation 
](https://pytorch.org/tutorials/advanced/torch_script_custom_ops.html#building-with-jit-compilation). Meaning that the CUDA/C++ code is compiled transparently the first time DistanceCellList is instantiated in a way compatible with the current "pip install -e ." workflow.
If an user does not use the new module, nothing is compiled and no additional dependencies/overhead are required.

OTOH, a user constructing DistanceCellList will:
1. Require NVCC installed with a CUDA version ABI compatible with torch (cudatoolkit-dev from conda-forge works)
2. Experience a several minutes compilation time the first time DistanceCellList is used after "pip install -e ." (torch will cache the compilation).
 
### Tasks:
- [x] Adapt the neighbor functions in NNPops to torchmd-net "pip install -e ." installation (thanks #61).
- [x] Implement a batch-aware cell list neighbor construction
- - [x] Optimize the cell list.
- - [x] Make the operation CUDA graph compatible
- [x] Add tests
- [x] Add benchmark 
- [x] Feature parity with current ```Distance``` module.
- - [x] Add the "loop" parameter (include self-interactions)
- - [x] Add the "cutoff_lower" parameter
- Add periodic boundary conditions
- - [x] Support rectangular boxes
- - [ ] Support triclinic boxes
- - - [x] Brute force 
- - - [ ] Cell list
- [x] Add backwards pass
- - [x] CPU (Autograd takes care of it)
- - [x] GPU (common backwards pass for every strategy)
- [x] torch.jit.script compatibility

### Challenges:

1. The brute approach cannot handle more than 32K particles total. AFAIK, there is no way to make it work without destroying what makes it be so fast. Anyhow, this strategy is not really suited for such high workloads. There is a guard that simply forces the shared strategy to be used if the user selected brute but more than 32K particles are requested.
2. The performance of the cell strategy degrades quickly with the number of batches. This is because I construct a single cell list such that particles with the same cell are contiguous in memory. When traversing the cell list, one thus finds particles from all batches, forcing a lot of unnecessary checks. I mitigate this by sorting by batch inside each cell, allowing to skip some pairs, but this could be done in a more smart way, I am sure (maybe a binary search looking for the first atom in the current particle`s batch?).
The alternative, constructing a cell list per batch, requires much more memory and cannot be done without GPU-CPU memory copies.  
3. Automatically choosing a strategy based on some heuristic. I tried this in a million ways, but jit.script is not taking it.  The heuristic cannot be applied until the forward method (when positions and batch are known), changing the function dynamically like that is just not something that TorchScript supports as far as I can tell.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9b923920426d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.