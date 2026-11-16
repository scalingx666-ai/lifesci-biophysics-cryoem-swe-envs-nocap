# Issue

**Title:** So3lrEngine: energy and forces for MD codes

## Description

## So3lrEngine: energy and forces for MD codes

Follows #63 (merged), which moved jax-md to current upstream and skips jax 0.10.2, on which the CPU tests here fail.

`So3lrEngine` is a small public API for codes that call SO3LR as a force callback, such as OpenMM, i-PI, LAMMPS or a hand-written loop. The atoms, charge and spin are fixed when it is built; each call takes positions and an optional box and returns the energy and forces.

```python
import jax
jax.config.update("jax_enable_x64", True)        # mixed (default) and float64 need it
from so3lr import So3lrEngine

engine = So3lrEngine(numbers, model="so3lr-tauri-m", charge=0, multiplicity=1)
energy, forces = engine(positions, box)          # Å, 3×3 lattice rows in Å or None → eV, eV/Å
```

For each box it uses the first of so3lr's three backends that can compute it:

| Backend | Covers | Notes |
|---|---|---|
| flash | orthorhombic boxes ≥ 2× the cutoffs, or no box, on a supported GPU | the same check (`flash_problem`) as `so3lr md` / `so3lr eval` |
| neighbour lists (`to_jax_md`) | orthorhombic boxes ≥ 2× (cutoff + skin), or no box | one compiled step; lists update on the device and are reallocated when they overflow or the box shrinks below their cells, with more room for each overflow in a row, as in `so3lr md` |
| exact images (`So3lrCalculator`, `neighbor_backend="exact"`) | any box, including triclinic and thin cells | list built on the CPU each call |

`engine="auto" | "flash" | "standard"` works as `--engine` does for the CLI: auto also falls back, with a warning and for the rest of the engine's life, where flash does not import or fails (to start, to be rebuilt for a moving box, or to lay out a new box). Flash is checked once per engine for the settings, model and device, and per box for the structure; only the last box's choice is kept, so a barostat's new box at every step costs nothing extra. Flash is built for the first box without NPT's margin and is rebuilt once with it when the box first moves. A box of zeros means free space, and a box with no volume is refused. Other arguments: `precision`, `lr_cutoff`, `dispersion_damping`, `kspace_electrostatics` (`ewald`, `pme` or `p3m`, with smearing and spacing), `skin` and `matmul_precision`.

The engine pickles as its constructor arguments, so a host that serialises its force objects (OpenMM's `XmlSerializer`, for example) gets a copy that recompiles on its first call.

### Why

Callers outside so3lr currently reimplement this logic on top of internal helpers (`to_jax_md`, `minimum_image_problem`, `setup_kspace_grid` and the neighbour-list error bits), and any release can break them. With this class, an OpenMM-ML wrapper (a follow-up, in openmm-ml) keeps only the OpenMM side: building the engine from the topology and passing positions and box.

### Tests

`tests/test_engine.py`, added to the `so3lr-other` CI lane, compares the engine with `So3lrCalculator` in float64 on:
- a molecule, neutral and charged;
- 64 waters, with positions and box changing between calls;
- the same box with PME and with P3M;
- a thin cell and a triclinic cell, both of which take the exact backend;
- an overflowing neighbour list (only the overflowing list is reallocated);
- one engine going between free space and a box;
- a box of zeros, and a flat box;
- a flash engine that fails under auto, at first or when rebuilt for a moving box (flash then stays off), and the same failure raising under `engine="flash"`.

It also covers pickling and refused arguments. On a GPU it compares flash with the neighbour lists, with no box, and with a box that moves. On an A100 all of them pass, on the CUDA kernels #62 brought to compute capability 8.0. On CPU (jax 0.10.1) all pass except the GPU-only ones.

### Not included

- **k-space dispersion:** the exact backend (the ASE calculator) has none.
- **Stress:** not computed yet.
- **Docs:** a page for the API on so3lr.github.io follows in the docs repo.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c26c6fb3cd4f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.