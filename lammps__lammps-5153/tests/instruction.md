# Issue

**Title:** KOKKOS: sync per-atom data on demand during variable evaluation

## Description

**Summary**

Alternative to 

The KOKKOS versions of `fix setforce`, `fix addforce`, and `fix efield` each called `atomKK->sync(Host,ALL_MASK)` before evaluating their force variables, because variable evaluation happens on the host.  Copying every per-atom array back from the device on every step is far more than these formulas need.  Each of those calls carried a `// this can be removed when variable class is ported to Kokkos` comment.

Rather than make every caller sync defensively up front, the `Variable` class now requests the data itself at the points where it reads per-atom arrays on the host.  The functions that do so become virtual, and `VariableKokkos` overrides each one to make just the data it needs current and then delegate to the base class:

| seam | what it reads | synced |
|---|---|---|
| `compute_atom()` | `atom->mask` for the group test | `MASK_MASK` |
| `atom_vector()` | one named atom vector | `X`/`V`/`F`/`Q`/`TYPE`/`TAG`/`MOLECULE`/`RADIUS`/`RMASS` |
| `group_function()` | `xcm()`, `fcm()`, `gyration()`, ... | `ALL_MASK` |
| `special_function()` | `gmask()`, `rmask()`, `grmask()` | `MASK`, `X`, `X\|MASK` |
| `peratom2global()`, `custom2global()` | a single atom by ID | `ALL_MASK` |

Per-atom data reached through a compute, fix, or custom property has no such seam, since `evaluate()` handles it inline.  Those three branches call a small virtual `sync_peratom()` instead, which is the only hook left in `variable.cpp`.

The result is that all knowledge of the bitmasks lives in `VariableKokkos`; `atom_masks.h` is not included by `variable.cpp` at all.  The net addition to `variable.cpp` is three one-line calls plus the extraction of `is_group_function()` described below.  `Input` instantiates `VariableKokkos` when KOKKOS is active.  The three fixes drop their blanket sync with no replacement, and every other caller of an atom-style variable benefits automatically -- `variable->compute_atom()` is called from more than fifty files.

**Related Issue(s)**

This is an alternative approach to and thus .

**Author(s)**

Stan Moore, Sandia National Laboratories

**Licensing**

By submitting this pull request, I agree, that my contribution will be included in LAMMPS and redistributed under either the GNU General Public License version 2 (GPL v2) or the GNU Lesser General Public License version 2.1 (LGPL v2.1).

**Artificial Intelligence (AI) Tools Usage**

This code was created by Claude Code, under my direction and review

<!-- to be completed by the author before this is opened upstream -->

**Backward Compatibility**

No backward compatibility is broken.  There are no user visible changes; this only affects when per-atom data is copied back from the device in KOKKOS runs.

**Implementation Notes**

Changes to the `Variable` class interface: `compute_atom()` becomes virtual, and `Tree`, `atom_vector()`, `group_function()`, `special_function()`, `peratom2global()`, and `custom2global()` move from private to protected so the subclass can override and forward.  The subclass never touches the members of `Tree`; it only passes the pointers through.  Nothing else in `Variable` is exposed -- `evaluate()`, `collapse_tree()`, `eval_tree()`, and the variable list stay private.

The name to mask mapping is deliberately fail-safe.  `atom_vector()` starts at `ALL_MASK` and only narrows, so an atom vector added to the base class without a matching entry in `VariableKokkos` syncs more than it needs to rather than reading stale data.  The compute, fix, and group function paths request `ALL_MASK` for the same reason.

Three cases need care and are handled explicitly:

- `mass` without per-atom `rmass` is a per-type array that `eval_tree()` indexes with `atom->type[i]`, so it needs `TYPE_MASK` rather than `RMASS_MASK`.
- `peratom2global()` reads the named vector itself (`x[100]`, `vy[7]`) in addition to the atom map, so it cannot be narrowed to `TAG_MASK`.
- `group_function()` is tried for every function word in a formula and returns 0 for anything that is not a group function.  Syncing unconditionally in the override would therefore sync everything on `gmask(...)`, `rmask(...)`, and every special function, undoing most of the benefit.  The guard is factored out of the base class as `is_group_function()` and shared, so there is one copy of that word list rather than two that can drift.

Custom per-atom properties are identified by the `i_` / `d_` / `i2_` / `d2_` prefix and map to `IVECTOR_MASK`, `DVECTOR_MASK`, `IARRAY_MASK`, and `DARRAY_MASK`.

The `modify_host()` and `sync<DeviceType>()` calls on the result arrays in the three fixes are unchanged and stay.  Those are a real host to device copy of the variable result, not a stale flag: the value is computed on the host and consumed by a device kernel, so the bytes have to move.  That can only go away if variables are evaluated on the device.  The comments claiming they could be removed once the variable class was ported have been corrected.

Verification performed so far, all on a CPU-only (Serial) KOKKOS build:

- `VariableSyncTest.AcceleratorSeams` swaps in a `Variable` subclass that records which seams a formula routes through, and asserts them for the core atom vectors, `gmask`/`rmask`/`grmask`, indexed per-atom access, custom properties, and the compute and group function fallbacks.  The full `test_variables` suite passes (11/11).  This test is what caught the `group_function()` over-sync described above.
- A `fix addforce` / `fix setforce` input using only narrow formulas (`x`, `type`, `id*q`, so that `V_MASK` and `F_MASK` are deliberately not synced) agrees between a host run and `-k on -sf kk` to 6.4e-12, against a 7.5e-11 baseline from a control deck that uses no variables at all.
- `make check-whitespace` and `make check-permissions` are clean.

One limitation worth stating plainly: on a CPU-only KOKKOS build `LMPDeviceType` and `LMPHostType` are the same type, the dual views alias, and `atomKK->sync(Host,...)` is a no-op.  The tests above therefore establish which seams a formula uses and that formula evaluation is unaffected, but they cannot establish that the set of syncs is *sufficient*.  A missed seam would pass these tests and still produce stale data on a GPU build.  This needs a GPU regression run before it is merged.

**Post Submission Checklist**

- [x] The feature or features in this pull request is complete
- [x] Licensing information is complete
- [x] Corresponding author information is complete
- [x] The source code follows the LAMMPS formatting guidelines
- [x] The feature has been verified to work with the CMake based build system
- [x] Suitable tests have been added to the unittest tree.

**Further Information, Files, and Links**

New files are registered in both `src/KOKKOS/Install.sh` and `cmake/Modules/Packages/KOKKOS.cmake`.  `src/.gitignore` already covers `*_kokkos.cpp` and `*_kokkos.h` by wildcard, so no change was needed there.

`fix_langevin_kokkos` and `pair_hybrid_scaled_kokkos` also call `compute_atom()` but did not perform the blanket host sync, so they were left alone; they may deserve a separate look.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `bac6be8f29ca`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.