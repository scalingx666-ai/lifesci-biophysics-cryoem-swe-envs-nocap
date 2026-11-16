# Issue

**Title:** feat(flash): hold fixed_atom_indices in place in NVE/NVT MD

## Description

Flash declined every run with `fixed_atom_indices`, so a solute held in a periodic water box fell back to the neighbour-list engine.

- `so3lr/flash/md.py`: after every step of `nve`, `nvt` and `nvt-langevin`, the held atoms go back to their start positions with zero momentum. Their initial momenta are zeroed too.
- `so3lr/flash/check.py`: `fixed_atom_indices` is handled for MD. Flash still declines it, with a reason, for NPT, for `opt`, `eval` and the ASE calculator, and when the relaxation before MD would run (a restart skips it). `atom_mask_path` and `mobile_atom_indices` stay unsupported, with a pointer to `fixed_atom_indices`.

Tests (`tests/test_flash.py`): engine-choice cases for these rules, a restart case, and a GPU test of `nvt-langevin` on flash in which the held atoms stay in place with zero momentum while the rest of the box moves.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `363c84437845`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.