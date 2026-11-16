# Issue

**Title:** KOKKOS: add GPU-aware Pair forward/reverse comm to CommTiledKokkos

## Description

**Summary**

KOKKOS: add GPU-aware Pair forward/reverse comm to CommTiledKokkos

**Related Issue(s)**

Fixes 

**Author(s)**

Stan Moore (SNL) and Claude Code (Opus 4.8)

**Licensing**

By submitting this pull request, I agree, that my contribution will be included in LAMMPS and redistributed under either the GNU General Public License version 2 (GPL v2) or the GNU Lesser General Public License version 2.1 (LGPL v2.1).

**Artificial Intelligence (AI) Tools Usage**

I used Claude Code (Opus 4.8)

**Backward Compatibility**

Yes

**Implementation Notes**

Description from Claude:

## What changed

`CommTiledKokkos::forward_comm(Pair*)` and `reverse_comm(Pair*)` previously always delegated to the CPU `CommTiled` base class, whose `pack/unpack_forward_comm()` routines dereference per-atom pointers on the host. For a Kokkos pair style resident on the GPU, those are device pointers, so the host-side dereference segfaults during dynamics (issue #4942). The crash stays latent at step 0 and surfaces once atoms move (e.g. step 1 of NVT).

This PR brings `CommTiledKokkos` in line with what `CommKokkos` (`comm_style brick`) already does for pair comm:

- **New device path.** Added templated `forward_comm_device<DeviceType>(Pair*, int)` and `reverse_comm_device<DeviceType>(Pair*, int)` that pack/unpack on the device via `pack_forward_comm_kokkos()` / `unpack_forward_comm_kokkos()` (and the reverse equivalents), preserving the tiled **multi-proc-per-swap** pattern with per-proc recv offsets (offset subviews of the dual-view buffers).
- **Dispatch matches `CommKokkos`.** `forward_comm(Pair*)` routes to the device path unless `execution_space == Host/HostKK` or `forward_pair_comm_legacy`; `reverse_comm(Pair*)` additionally checks `!pair->reverse_comm_device`. Host/legacy pairs keep the exact previous behavior (`k_sendlist.sync_host()` then the CPU base call).
- **Dedicated pair buffers.** Added `k_buf_send_pair` / `k_buf_recv_pair` sized by `grow_buf_pair()`, which also addresses the buffer-undersizing concern for pair comm at the Kokkos layer (rather than in `comm_tiled.cpp`).
- **Non-GPU-aware MPI** supported by staging through the host mirror of the dual-view buffers, matching `CommKokkos`.

No pair styles were modified — the fix is entirely in `comm_tiled_kokkos.{h,cpp}` and benefits every Kokkos GPU pair style that uses pair forward/reverse comm with `comm_style tiled`.

## Verification

**Code review** (3 independent finder passes — line-by-line correctness, removed-behavior/cross-file, cleanup/altitude — each with a verifier): no correctness defects. Dispatch conditions, buffer-offset units, host/device sync ordering, the self-comm path, and `MPI_Waitall`/request sizing were all confirmed against the `CommKokkos` reference and the CPU `CommTiled` algorithm.

**Build:** full KOKKOS (Serial) + MPI build links cleanly; the changed TU compiles with no new warnings.

**Functional tests** — 4 MPI ranks, `comm_style tiled`, NVT/NVE dynamics. Energy trajectories compared bit-for-bit against the CPU tiled path, the brick device path, and pure non-Kokkos:

| Pair style | Exercises | Result |
|---|---|---|
| `eam/kk` | `forward_comm_device` | identical to all reference paths |
| `meam/kk` (`reverse_comm_device=1`) | `forward_comm_device` + `reverse_comm_device` | identical to all reference paths |
| `mliap/kk` **unified** | `forward_comm_device` + `reverse_comm_device` | identical to host/brick references |

Both GPU-aware buffer branches (`gpu/aware on`/`off`) and the single-rank self-comm path were also exercised, all matching ground truth.

**ML-IAP note (issue's actual style).** The mliap pair comm path is reached *only* through the `mliap unified` interface — `forward_exchange`/`reverse_exchange` are invoked solely from `mliap_unified_couple_kokkos.pyx`; linear/quadratic SNAP models never communicate descriptors. So verification was done by building with `MLIAP_ENABLE_PYTHON` and running a unified model (PyTorch tensors) that drives the exchanges. Instrumentation confirmed `forward_comm_device`/`reverse_comm_device` for `Pair=mliap/kk` are entered on all ranks; the host path showed zero entries (confirming the dispatch gating). Per-rank checksums of the exchanged buffers from the new device path were byte-identical to the CPU reference across all ranks and steps, and the forward exchange filled 100% of ghost entries.

**Caveat:** all functional testing was on a CPU/Serial Kokkos build, where device memory == host memory, so the original GPU segfault itself cannot be reproduced without a GPU. What this verifies is that the new device pack/unpack path — the code that runs on the GPU and replaces the offending host dereference — is entered in the right configurations and is numerically correct. Confirmation on actual GPU hardware is still worthwhile before merge.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `be18f08d690b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.