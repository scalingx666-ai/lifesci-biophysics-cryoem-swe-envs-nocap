# Issue

**Title:** SO3LR-Flash: CUDA kernels on GPUs with less shared memory per block

## Description

## SO3LR-Flash: CUDA kernels on GPUs with less shared memory per block

### Summary

The CUDA kernels from #59 and #60 ran only on GPUs of compute capability 9.0, because their
attention tiles were sized for 227 KiB of shared memory per block. This PR sizes the tiles
from the GPU's own shared memory instead. The kernels now run on any NVIDIA GPU of compute
capability 8.0 or newer that can hold a model's smallest tiles. Where the old tiles already
fit, nothing changes.

It also includes two follow-ups from #60:
- re-planning memory after the neighbour rows widen;
- a clearer error for `flash_kernels cuda` when nothing fits.

### What changes

**Tile sizes come from the GPU**
- The build reads three limits from the CUDA driver: shared memory per block, per SM, and
  reserved per block.
- For each model it picks the largest tiles that fit, in this order: 64, 32 or 16 pairs per
  tile; two pipeline stages before one; for the backward kernel, all channels in one block
  before splitting them across two.
- It prefers tiles that let an SM hold as many blocks as the kernel is written for. That
  number is now a build flag, so smaller tiles do not spill registers.
- A `static_assert` in each kernel checks that its shared memory matches the Python count,
  so the two cannot drift apart.
- No GPU model is named in the code.

**Which models fit**

| Shared memory per block | so3lr-tauri-s/m | so3lr-tauri-l |
|---|---|---|
| 227 KiB | unchanged: same tiles, same compiled code, same speed | unchanged |
| 163 KiB | 32 pairs per tile | 16 pairs per tile |
| 99 KiB | 64 pairs per tile forward, 32 backward | does not fit: runs on the Pallas kernels, with a log line saying why (`flash_kernels cuda` raises) |

**Other changes**
- **FFI target names** include the GPU architecture and the tiles, so the persistent
  compilation cache never mixes two builds.
- **After the neighbour rows widen** (), the kernels plan again in the
  memory they were first planned in. They keep every layer's per-pair data while it still
  fits, and switch to chunks of rows when it does not. Before, a system near the top of the
  kept range could run out of memory after a neighbour overflow.
- **`flash_kernels cuda` when nothing fits** (): if no CUDA mode fits and
  the memory mode that does is one only the Pallas kernels have, `md` now says why at the
  start, instead of failing later inside the kernels.
- **Tests that run `so3lr` as a subprocess** now let it allocate GPU memory as it goes, as
  the two-GPU test already did. On GPUs with less memory the subprocess otherwise has no
  room beside the test process.

### Accuracy

| Comparison | Energy | Forces |
|---|---|---|
| Kernels built with the 163 KiB and 99 KiB tiles vs the current build | within 6e-10 eV/atom | within 3e-6 eV/Å (rms below 5e-7) |
| On a GPU with 163 KiB per block, vs the Pallas kernels | — | rms 2–3e-5 eV/Å |

Both comparisons use a 55k-atom water box with PME and all three models, with the per-pair
data kept, in 3 chunks and staged. The second row's difference is the Pallas kernels' own
float32 error against the float64 engine (#59).

### Speed

On a GPU with 163 KiB per block: the MD engine with `flash_kernels: auto` against `pallas`,
time per force call, water with PME.

| Model | Atoms | Speedup over Pallas |
|---|---|---|
| so3lr-tauri-m | 55k – 1.1M | 2.0× (in chunks from 331k) |
| so3lr-tauri-s | 331k | 2.0× |
| so3lr-tauri-l | 55k | 2.1× |
| so3lr-tauri-l | 331k | 1.65× |

No out-of-memory errors at any size. The 99 KiB tiles are checked for accuracy but have not
been timed on such a GPU.

### Tests

New in `tests/test_flash.py`:
- the tiles picked for 227, 163 and 99 KiB per block (runs on CPU);
- kernels built for compute capability 8.0 with the 163 KiB and 99 KiB tiles, against this
  GPU's build (so3lr-tauri-m and -l, kept and in chunks; -l declined at 99 KiB);
- the switch to chunks after the neighbour rows widen in tight memory;
- the `flash_kernels cuda` error when nothing fits.

The CUDA tests are skipped without a GPU of compute capability 8.0 or newer and nvcc.

### Follow-up

With 163 KiB per block, so3lr-tauri-l uses 16-pair tiles, which costs part of its speedup.
Splitting its heads across two blocks would let it use 32-pair tiles.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ee62fa288608`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.