# Issue

**Title:** feat(flash/cuda): the CUDA kernels in chunks of rows, for systems of millions of atoms

## Description

## SO3LR-Flash: the CUDA kernels in chunks of rows, for systems of millions of atoms

Builds on #59. The CUDA kernels there keep every layer's per-pair data (filters and scores)
from the forward for the backward, which takes 100-400 KiB per atom: on a single GPU they
stopped at about a million atoms (so3lr-tauri-s/m) or a few hundred thousand (so3lr-tauri-l),
and larger systems ran on the Pallas kernels. This PR makes them serve systems of tens of
millions of atoms.

### What changes

- **Chunks of rows.** Where the per-pair data does not fit, the kernels make it a chunk of
  rows at a time: in the forward for the scores (the messages of the chunk's rows), and again
  in the backward for the filters and scores of the chunk's pairs, its pairs' backward and its
  rows' gathers. A pair with a partner outside the chunk is made by both chunks, as by two
  devices. The per-slot and per-pair buffers are now sized by the range of rows they serve
  (a device's or a chunk's), with range-local slots, so on several devices each one holds its
  own rows' only. The kept-data path is unchanged and still used wherever it fits.
- **In the engine's memory modes.** In chunks, the kernels replace the engine's attention
  (forward, and the backward pass) rather than its layers, so the engine's own machinery
  does the rest: the post block and heads a tile of rows at a time, dE/dx_in in place, the
  staged programs, and the modes that make the layers' attention outputs or inputs again.
- **Picked by memory.** For `flash_mode: auto` the MD picks the fastest CUDA mode that fits
  (per-pair data kept; else in chunks, jitted, then staged), and the Pallas kernels' modes
  beyond. The kernels' memory model (per mode: the top layer's backward and what the mode
  keeps of the layers below, plus a chunk's buffers) follows the measured peaks of the three
  models within a few percent; the chunk size follows the free memory, and is made again from
  the same budget after the neighbour rows widen. In the staged modes a stage's temporaries
  are one allocation; the planner keeps the largest under 40% of the free memory, as larger
  ones were not found in one piece after the first MD steps.
- **Atoms in a spatial order.** Each force call runs with the atoms sorted along a Morton
  curve of the box (the forces given back in the input's order; `FlashSO3LR(sort_atoms=)`),
  so a chunk's, a device's and the kernels' neighbour rows stay local whatever the input's
  atom order. Atoms in random order cost 16% with the per-pair data kept and 72% in chunks
  before; now the same as an ordered input. This applies to the Pallas kernels too.

### Speed and memory

On a periodic water box, MD (NVT, PME), against the Pallas kernels on the same GPU:
so3lr-tauri-s and -m run on the CUDA kernels up to 16 million atoms and so3lr-tauri-l up to
9 million (before: about 0.8 million, and 0.3 million for -l), at 2.5-2.7x (S, M) and
2.2-2.8x (L) the Pallas kernels' speed in chunks, with a flat cost per atom. In chunks the
cost per atom is 1.2-1.3x that of keeping the per-pair data. Memory in chunks is about the
Pallas fast mode's (staged: about two thirds of it) plus a chunk's buffers.

### Accuracy

In chunks, every mode (jitted and staged, each memory mode, one GPU and two) against the
kernels keeping the per-pair data, 55k-atom water with PME, all three models: energies
within 1e-9 eV/atom on one GPU (1e-8 on two), forces within 1e-5 eV/Å (rms below 1.5e-6;
so3lr-tauri-s/m on one GPU within 5e-6). That is the size of the difference between one
GPU and two with the per-pair data kept (up to 8e-9 eV/atom and 1.4e-5 eV/Å, as in #59).
With the atoms in their input order, the kept path gives the same results as in #59 to
1e-9 eV/atom and 7e-6 eV/Å (1e-6 rms).

### Tests

`tests/test_flash.py`: the CUDA kernels in chunks (forced small chunks, the last one shifted)
against the kept per-pair data for so3lr-tauri-m in three memory modes, -s and -l, jitted,
staged and on two GPUs; the atoms in random order against their own order (float64, jitted
and staged); the fallback test now names the modes that stay on Pallas (those splitting the
attention into passes). Skipped without an sm_90 GPU and nvcc, as in #59.

### Follow-up

The modes that make the attention outputs or the layer inputs again run on the kernels when
asked for (`flash_mode`), but `auto` does not pick them: on the systems they are meant for,
the stage holding every atom's projections is a single allocation of most of the GPU. Making
the projections a stage of their own would let them reach further.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d235d6d60430`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.