# Issue

**Title:** Convert a System's float type, units and device with the copy constructor

## Description

## System conversion: float type, units and device

Migrated a lot of functionality that I had in the development branch for my force field refinement project. These were originally several method overloads, but Claude suggested centralising it, and I actually think it is quite a good idea, definitely more user-friendly.

`System(sys; float_type, units, array_type)` now converts the system's data. Previously, changing `float_type` only changed the type parameter and left the data in the old precision, and there was no way to strip units or change device for a whole system.

```julia
# e.g. Float32 + units on GPU → Float64, unitless, CPU, for Enzyme
sys_cpu = System(sys; float_type=Float64, units=false, array_type=Array, grad_safe=true)
```

### Behaviour
- **New copy-constructor keywords:** `units::Bool` (default: whether `sys` has units) and `array_type` (default: the system's).
- **When it converts:** only if `float_type`, `units` or `array_type` differs from `sys`. Otherwise the constructor runs the existing code path, unchanged.
- **`float_type_high` default:**
  - on the same device: `promote_type(float_type, float_type_high(sys))`;
  - on a new device: that device's default, so backends without Float64 still work.
- **Converted:** atoms, coords, velocities, boundary, all interactions, constraints, virtual sites, neighbor finder and `k`. Keyword arguments passed alongside a conversion are converted too.
- **Not converted:** `atoms_data`, `topology`, `data`.
- **Loggers** are dropped unless passed explicitly, since their histories and observables are tied to the original types and device.
- **Units** are stripped with plain `ustrip`, so values stay in the units they were stored in. Adding units to a unitless system throws.

### Implementation (`src/conversion.jl`)
- `Molly.convert_component(::SystemConversion, x)` is the extension point for custom types.
- **Generic path:**
  - floats become `T`; quantities become `T`, with or without their units;
  - stored `Units` become `NoUnits` when stripping (e.g. `MullerBrown`);
  - tuples, named tuples and static arrays are mapped;
  - other arrays are mapped on the CPU and moved to the target device if their eltype is bits;
  - any other struct is rebuilt with `typeof(x).name.wrapper(fields...)`.
- **Safety check:** after a rebuild, the new struct's fields must equal the converted inputs. If the rebuild fails or a constructor reinterprets its arguments, an `ArgumentError` naming the type is thrown.
- **Explicit methods:**
  - `Atom`, `TriclinicBoundary`;
  - `PME`, `PME_λ`: rebuilt through their constructors for grids, buffers and FFT plans, keeping `α` and the λ mesh weight;
  - implicit solvent (`AbstractGBSA`): parameters converted, buffers set up again for the target device and thread count;
  - `SHAKE_RATTLE`, `LINCS`, `AngleConstraint`;
  - neighbor finders: rebuilt from their stored `eligible`/`special` matrices, so sparse stays sparse;
  - `ANIPotential` throws.
- **Neighbor finders:**
  - on the same device the finder type is kept if the new system can use it;
  - otherwise (a device change, or `GPUNeighborFinder` with `grad_safe=true`) the finder that default system setup would choose is used.
  - Moving a system to another device and back gives the original finder type.
- Every type in `src/interactions` either converts or throws.

### Tests (`test/conversion.jl`)
The conversion touches every part of a `System`, so the tests use one system per distinct code path, each on CPU and GPU:
- water PME (TIP4P-FB, virtual sites);
- DEXP + reaction field (a custom non-bonded form);
- CHARMM protein PME with LINCS (CMAP, Urey–Bradley);
- benzene Ewald with SHAKE;
- protein with GBN2 implicit solvent;
- triclinic water.

Running every system on the GPU caught a real bug: implicit-solvent buffers depend on the device and thread count and must be rebuilt on a device change, which the CPU-only energy checks missed.

- **Precision and units:**
  - Float64 with → without units: energy at `rtol=1e-12`, forces at 1e-10 × the largest force;
  - Float32 → Float64: values widen exactly, and a round trip to Float32 reproduces the original energy and forces;
  - Float64 → Float32: values round exactly, giving the atoms of a system set up in Float32.
  - Energies computed in Float32 are compared with a per-system tolerance, because the limit is the Float32 calculation's own error, not the conversion: 1e-6 by default; DEXP 1e-5, Ewald 5e-4, triclinic water (9 atoms) 2e-5, GPU implicit solvent 1e-5.
- **Devices:**
  - GPU → CPU: values widen exactly;
  - CPU → GPU: the result matches a system set up on the GPU (neighbor finder, `float_type_high`, degrees of freedom, energy).
- **Neighbor finders:**
  - `Distance`, `Tree` and `CellListMap` are kept on the CPU;
  - `GPUNeighborFinder` is replaced when `grad_safe` is set;
  - `GPUCellListNeighborFinder` is kept on the GPU and replaced on the CPU.
- **Simulation:**
  - a converted unitless system follows the original trajectory for 20 steps (virtual sites, LINCS, neighbor list updates);
  - a system moved to the GPU follows one set up there (SHAKE, virtual sites).
- **Hand-built systems:** a custom atom type on CPU and GPU; a 2D system with an infinite boundary and `MullerBrown`.
- **Alchemical:** an `AbsoluteFESystem` with the GROMACS scheduler (`PME_λ`, soft core and λ bonded interactions).
- **Per type:**
  - every interaction type in `src/interactions` converts with exact values;
  - an unconvertible struct and `ANIPotential` throw errors naming the type.
- **Keywords:** loggers, conversion of passed keyword arguments, the `float_type_high` default, the no-conversion path, adding units.
- **Enzyme:**
  - parameter gradients on a converted system match finite differences;
  - gradients through the rebuilt gradient-safe PME match the forces.

Existing `basic`, `simulation_1`, `simulation_2`, `gradients_2`, `protein_1`, `free_energy` and `aqua` pass. The GPU tests ran on CUDA; other GPU backends were not tested.

Docs: units section of `documentation.md` and the `System(sys; ...)` docstring.

🤖 Generated with help of [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `623d314aad49`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.