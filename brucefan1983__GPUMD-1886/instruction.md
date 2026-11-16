# Issue

**Title:** dftd3: fix allocation when switching to the large-box path

## Description

### Summary

`DFTD3::compute` selects the small-box or large-box path on every force call. The small-box path is used when a periodic thickness is at most 2.5 times the D3 cutoff.

`compute_large_box` currently checks `cn.size() == 0` to decide whether to allocate its buffers. However, `cn` is shared with the small-box path. After a small-box calculation, a switch to the large-box path therefore skips initialization, and `find_cell_list` calls `cudaMemset` on the unallocated `cell_contents` array. Master stops with a CUDA error (code 1, invalid argument) at `force/neighbor.cu:193`.

The reverse switch works because `compute_small_box` checks `NN_radial`, which is only used by that path.

The switch can be triggered by `change_box` between two `run` commands, or by a barostat or `deform` moving the box across the threshold. With `dftd3 pbe 12 6`, the threshold is 30 Å.
### Changes

- `compute_large_box` now checks `cell_contents_radial.size() == 0` instead, since that array is specific to the large-box path. This is one line in `dftd3.cu`.

### Validation

I compared master `8bf53010` with this branch on an RTX 4090, using CUDA 12.2, `CFLAGS="-std=c++14 -O3 -arch=sm_89 -DDEBUG"`, and D3 (PBE) with NEP89.

Four fully periodic structures were tested (33, 90 and 564 atoms with 12/6 cutoffs; 5440 atoms with 20/12). Each structure had two states on opposite sides of the threshold. I ran both states directly and alternated between them in five-segment sequences, starting from either state (`time_step 0`, two steps per segment).

- Master fails in all four sequences starting on the small-box path, at the first force call after the switch. On one of them, compute-sanitizer gives the backtrace `cudaMemset` ← `find_cell_list` ← `DFTD3::compute_large_box`.
- This branch completes every run. Each segment agrees with the corresponding direct run within 1.2e-6 meV/atom in energy, 1.0e-6 eV/Å in force and 4.3e-5 meV/atom in virial.
- For the 28 of 32 runs that master completes, the branch agrees within 1.1e-6 meV/atom, 1.6e-7 eV/Å and 1.0e-5 meV/atom, respectively.
- Compute-sanitizer memcheck and initcheck report zero errors on four branch sequences.
- `tests_regression full`, master against this branch, passes 255/255 cases and 25/25 relations.

Only `change_box` at fixed atom number was tested; I did not run a barostat trajectory across the threshold. Once both paths have been used, both sets of buffers remain allocated.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8bf530101c6a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.