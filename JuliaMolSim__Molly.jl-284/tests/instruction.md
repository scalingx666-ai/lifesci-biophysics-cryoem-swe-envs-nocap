# Issue

**Title:** ANI: fix GPU device selection so NN weights land on the data's device

## Description

## Summary

Fixes the ANI-2x `potential_energy`/`forces` failure on CUDA reported in the "GPU path from setup matches CPU" test:

```
ArgumentError: Objects are on devices with different types: CPUDevice and CUDADevice.   # energy
ArgumentError: Illegal conversion of a CUDA.DeviceMemory to a Ptr{Float32}              # forces
```

## Cause

`compute_ani_energy_ka` and `ani_energy_aev_grad_ka` selected the device for the on-device NN weights with `Lux.gpu_device()`. That relies on MLDataDevices' **GPU auto-detection**, which only considers a GPU "functional" when a trigger package (**cuDNN** / **LuxCUDA**) is loaded — not just `CUDA.jl`. The Molly test suite loads `using CUDA` but not cuDNN, so `Lux.gpu_device()` emits *"No functional GPU backend found! Defaulting to CPU."* and returns a `CPUDevice`. The NN weights then stay on the host while the AEVs are `CuArray`s, and the first `weight * aev` matmul throws.

(This is why it can pass in an environment that happens to have cuDNN/LuxCUDA loaded, and fail in the standard test env.)

## Fix

Pick the device from the **actual data array** with `Lux.get_device(aevs)` instead of the global `Lux.gpu_device()`. `get_device` reads the device off the array itself, works with only `CUDA.jl` loaded, and returns a stable-typed device object so the `ani_nn_dev_params` on-device weight cache (keyed on `typeof(dev)`) still works. Two lines, no API change.

## Verification

On an NVIDIA RTX 5080 with **only `using CUDA`** loaded (the failing config):
- Before: `Lux.gpu_device()` → `CPUDevice`, the errors above.
- After: `Lux.get_device(cuarray)` → `CUDADevice`; a small water molecule gives energy CPU vs GPU `|Δ| = 8e-6` and forces max-diff `3e-4` — GPU energy and forces match CPU.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e2a90137c8e2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.