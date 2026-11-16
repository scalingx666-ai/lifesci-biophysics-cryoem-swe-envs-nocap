# Issue

**Title:** dftd3: cache the reference weights per atom

## Description

### Summary

#1877 changed C6 interpolation to normalize reference weights for each atom. These weights depend only on the atom's coordination number, but the force kernels still recompute them for both atoms of every pair. As a result, NEP + D3 became about 18% slower with #1877.

This PR computes the weights once per atom in the coordination-number kernels. NEP + D3 runs 1.25–1.45× faster on the cases below, and results change only at rounding level.

### Changes

- `find_cn_weights` stores the normalized `w_i` and `dw_i = -8 w_i (d_i - <d>)` in `cn_w` and `cn_dw`, using 40 bytes per atom in total.
- `find_c6_cached` replaces `find_c6_stable`: atom 1's weights are loaded once per atom, outside the pair loop, while atom 2's are read from the cache. The C6 contraction and derivatives are unchanged; only rounding differs.
- The force kernels no longer read `cn`. I left its calculation in place to keep the diff small, but can remove it if preferred.
- Pairs where both elements have a single reference still use the existing shortcut and do not benefit from the cache.
- Only `dftd3.cu` and `dftd3.cuh` are modified.

The branch is rebased onto master after #1886. It differs from the code tested below only in the allocation check changed there.

### Validation

**Performance.** Median of four interleaved 2000-step NVE runs after warm-up, in atom·step/s:

| System | Atoms | Cutoffs | Path | GPU | Master | This PR | Speedup |
|---|---:|---|---|---|---:|---:|---:|
| 16 × (4 Na13 on graphene) | 9024 | 12 6 | large | RTX 4090 | 3.14e6 | 3.92e6 | 1.25× |
| | | | | A100 | 1.66e6 | 2.13e6 | 1.29× |
| Na in porous carbon | 16750 | 20 12 | large | RTX 4090 | 4.20e5 | 5.71e5 | 1.36× |
| | | | | A100 | 2.27e5 | 3.28e5 | 1.44× |
| Na in porous carbon | 3618 | 20 12 | small | RTX 4090 | 1.41e5 | 1.79e5 | 1.28× |
| | | | | A100 | 8.64e4 | 1.25e5 | 1.45× |

Run-to-run variation was below 0.5%. These measurements cover only the two GPUs listed above.

**Correctness.** Compared against master at `8bf53010`, using NEP89 on RTX 4090 (`sm_89`) and A100-PCIE-40GB (`sm_80`), both built with CUDA 12.2 and gcc 9.3.

- 61 static cases with 2–16750 atoms, covering both box paths and `12 6`/`20 12` cutoffs. Maximum differences from master:

| GPU | Energy (meV/atom) | Force component (eV/Å) | Virial (meV/atom) |
|---|---:|---:|---:|
| RTX 4090 | 2.6e-6 | 5.1e-7 | 2.1e-5 |
| A100 | 1.7e-6 | 4.8e-7 | 2.1e-5 |

- Both builds deviate from simple-dftd3 1.6.0 by at most 1.1e-3 meV/atom and 2.4e-6 eV/Å, and NEP-only results are identical. Two additional cases (A100 only) with no neighbors inside the coordination-number cutoff also agree within rounding.
- Three 10 ps NVE runs show energy drift of −0.0008 to −0.0022 meV/atom/ps on both builds and GPUs, comparable to NEP alone.
- `compute-sanitizer` memcheck and initcheck report zero errors on both paths and GPUs.
- `tests_regression full`: 254/255 cases and 25/25 relations pass, versus 255/255 for master against itself. The only failure is `dftd3_single_late`, with last-digit output differences of at most 6.6e-9 relative. Storing the weights after normalization changes the rounding.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d6cd2e361b47`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.