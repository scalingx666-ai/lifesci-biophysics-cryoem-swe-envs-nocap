# Issue

**Title:** SAM: prefer Apple MPS when CUDA is absent; fall back to CPU explicitly

## Description

.

## Summary

On Apple Silicon, SAM was forced onto the CPU even though the MPS GPU was available, because the device resolver only knew about CUDA. ViT-H mask generation over a large image then took minutes per call, the SAM fallback timed out, and the verifier kept re-prescribing it. With this change `auto` picks CUDA, then MPS, then CPU. If MPS cannot host or run the model, SAM falls back to the CPU explicitly, with a warning, instead of failing or silently defaulting to CPU.

## Technical description

- `particle_analyzer._resolve_device`: CUDA → MPS (`mps_available()`, a module-level helper tolerant of torch builds without the backend) → CPU.
- `_load_model`: placement failure on an accelerator falls back to CPU with a warning; a CPU failure still raises.
- `_run_sam`: MPS gaps surface at inference (unsupported op / dtype), not at placement — a `RuntimeError` / `NotImplementedError` / `TypeError` on an accelerator retries once on CPU, and the analyzer remembers the switch so later calls go straight to CPU.
- Tests (`tests/test_sam_device.py`, 8): every backend combination for `auto`, explicit devices honored, placement fallback + warning, CPU failure not swallowed, inference retry on CPU and the remembered switch.

Live check pending: running ViT-H on MPS on this machine needs the 2.5 GB checkpoint download.

### Live check (Apple Silicon, ViT-B checkpoint)

`auto` resolved to MPS. The first MPS inference failed with the exact caveat the issue anticipated — `Cannot convert a MPS Tensor to float64` from the automatic mask generator's float64 point grids — and the new fallback caught it and produced the same 34 raw masks on CPU. A follow-up commit casts the generator's point coordinates to float32 on MPS, after which the whole generator runs on the GPU with identical masks. A 2048² raster exceeded the MPS memory ceiling (9 GB on this machine) and fell back to CPU as designed. Timing runs were cut short to protect the machine's RAM, so no MPS-vs-CPU speed figure is claimed here.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `cbf8e2980900`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.