# Issue

**Title:** Keep <torch/torch.h> out of pair_schnetpack.h

## Description

## Problem

Building LAMMPS with the SchNetPack pair style fails during compilation of
`force.cpp` and `lammps.cpp`:

```
ATen/core/Generator.h:121:3: error: reference to 'Device' is ambiguous
  note: candidates are: 'LAMMPS_NS::ExecutionSpace LAMMPS_NS::Device'
  note:                 'struct c10::Device'
ATen/core/Generator.h:155:3: error: 'struct at::Generator' has no member named 'device'
ATen/core/TensorBase.h:587:41: error: too few arguments to function 'c10::TensorOptions c10::device(Device)'
```

...and several hundred more, all downstream of that first ambiguity.

## Cause

`pair_schnetpack.h` includes `<torch/torch.h>` at file scope. LAMMPS collects
every pair-style header into the generated `style_pair.h`, which `force.cpp`
and `lammps.cpp` include **after** their own `using namespace LAMMPS_NS;`.

libtorch's headers are therefore parsed with `LAMMPS_NS` injected into the
global scope, so the unqualified `Device` that ATen uses matches two
candidates:

| | |
|---|---|
| `c10::Device` | `c10/core/Device.h` — visible via `using namespace c10;` inside `namespace at` |
| `LAMMPS_NS::Device` | an enumerator of `enum ExecutionSpace { Host, Device }` in LAMMPS `src/pointers.h` |

Both resolve at global scope, so the lookup is ambiguous.

Note that only `force.cpp` and `lammps.cpp` fail. `pair_schnetpack.cpp` itself
compiles fine, because it includes its own header *before* the using-directive.
The bug is purely one of include order, which is why it does not show up when
building the pair style in isolation.

Also worth recording: the `ExecutionSpace` enum is **not** guarded by
`#ifdef LMP_KOKKOS` despite the comment above it, so disabling KOKKOS does not
avoid the clash.

## Fix

Hold the TorchScript module and its device behind an opaque pointer, defined in
`pair_schnetpack.cpp` — the only translation unit that includes libtorch. The
header then needs only `<memory>`.

```cpp
// pair_schnetpack.h
struct Impl;
std::unique_ptr<Impl> impl;

// pair_schnetpack.cpp
namespace LAMMPS_NS {
struct PairSCHNETPACK::Impl {
  torch::jit::script::Module model;
  torch::Device device = torch::kCPU;
};
}
```

The destructor is already defined out-of-line in the `.cpp`, which is what
`std::unique_ptr` to an incomplete type requires, so no change was needed there.

## What does not change

The pair style still requires and links libtorch. Only header *visibility*
moves — the same model is loaded onto the same device and run the same way.
Input script syntax, model files and results are unaffected. The overhead is
one pointer dereference per access, against a neural-network forward pass.

## Tested with

LAMMPS 22Jul2025_update5 (KOKKOS, OpenMP, MPI), PyTorch 2.13.0, GCC 15.2.0,
CUDA 13.2.1, C++17. Builds cleanly with the patch; fails without it.

## Related

The same collision affects other libtorch-based LAMMPS pair styles —
mir-group/pair_nequip_allegro#66 reports identical errors on GCC 11.4.1 /
PyTorch 2.6, and is still open. This suggests it started surfacing with newer
PyTorch headers rather than being compiler-specific.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `98af29905950`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.