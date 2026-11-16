# Issue

**Title:** GPU: add ewald/gpu k-space style

## Description

**Summary**

This pull request adds `ewald/gpu`, a GPU-accelerated version of the `ewald`
k-space style, to the GPU package. Unlike `pppm/gpu`, the Ewald method requires
no FFTs and therefore avoids the bandwidth-intensive 3D-FFT grid communication
of PPPM: the only cross-rank traffic is the `MPI_Allreduce` of the
`O(kcount)` structure-factor vector. While the Ewald sum scales worse with
system size than PPPM, this communication-light structure makes the GPU port
competitive with `pppm/gpu` for small to moderate system sizes and is a
generally valuable option to have.

The reciprocal-space structure factors, the per-atom forces, and the per-atom
energy and virial are all computed on the device and stay resident there
between phases. Only the small structure-factor vector is copied to the host
for the `MPI_Allreduce`, and the (cheap, `O(kcount)`) global energy/virial are
reduced on the host. Non-orthogonal (triclinic) boxes fall back to the existing
host code path.

The host side lives in `src/GPU/ewald_gpu.{cpp,h}` (subclassing `Ewald`) and
the device side in `lib/gpu/lal_ewald.{h,cpp,cu}` and `lal_ewald_ext.cpp`,
following the structure of the existing `pppm/gpu` and pair-style GPU code.

**Related Issue(s)**

None.

**Author(s)**

Axel Kohlmeyer (Temple University), akohlmey@gmail.com (corresponding author).

**Licensing**

By submitting this pull request, I agree, that my contribution will be included
in LAMMPS and redistributed under either the GNU General Public License version
2 (GPL v2) or the GNU Lesser General Public License version 2.1 (LGPL v2.1).

**Artificial Intelligence (AI) Tools Usage**

AI tools were used to generate code in this pull request. Specifically, the
Claude Code agent (Anthropic, Claude Opus 4.8) generated the majority of the
new C++/OpenCL source and documentation while working from a human-authored,
staged implementation plan. The human author directed the architecture and
design decisions, made all scoping choices, and performed and reviewed the
validation. This is consistent with the `Co-Authored-By` trailers on the
individual commits.

**Backward Compatibility**

No backward compatibility is broken. `ewald/gpu` is a new, optional accelerator
variant; existing inputs are unaffected.

**Implementation Notes**

Algorithm (mirrors the CPU `Ewald::compute()`), all device-resident for
orthogonal boxes:

- `k_ewald_cssn`: one thread per atom builds `cos`/`sin` of `m*unitk*r` for
  `m` in `[-kmax,kmax]` and all three directions via the standard recurrence.
- `k_ewald_structure`: one thread-block per k-vector reduces the local
  structure factors `sfacrl[k]=sum_i q_i cos(k.r_i)`,
  `sfacim[k]=sum_i q_i sin(k.r_i)` using a deterministic block reduction (no
  floating-point atomics, so results are reproducible across MPI rank counts).
  Accumulators use the accumulation precision (`acctyp`); `cs`/`sn` use the
  storage precision (`numtyp`).
- The host reduces the structure factors across ranks (`MPI_Allreduce`) and
  uploads the global values.
- `k_ewald_field`: one thread per atom sums the per-atom field over all
  k-vectors (reusing the resident `cs`/`sn`), writes the k-space force into the
  answer array (merged with the pair force by `fix gpu`), and optionally
  accumulates the raw per-atom energy/virial into separate device buffers; the
  host then applies the `q_i` factor, the self-energy correction, and `qscale`
  to match the CPU exactly.

Notes:
- A single `exprl/expim` reconstruction from the offset `cs`/`sn` arrays is
  shared by the structure-factor and field kernels; this reproduces the CPU's
  separate `eik_dot_r` loop nests without porting them.
- No CMake changes are needed: the `.cu` kernel has only the standard
  `numtyp`/`acctyp` precisions and is auto-globbed for OpenCL, CUDA, and HIP.
- `run_style verlet/split` and `fix balance` are explicitly rejected with a
  clear error; ranks with no local atoms contribute zero correctly.

Correctness (validated against `kspace_style ewald`, mixed precision, OpenCL):
global `E_long` agrees to ~2.5e-8 relative; per-atom forces to ~1e-6 max
relative; per-atom energy/stress to ~1.2e-4 (the expected single-precision
per-atom noise from k-sum cancellation, while the global sum still matches to
~2.5e-8); a triclinic box is bit-identical (host path); a slab system
(`kspace_modify slab`) agrees to ~2.3e-7; and a 2-MPI-rank run is bit-identical
to the serial GPU run.

Performance (2744 ions, accuracy 1e-4, mixed precision, OpenCL on an AMD
gfx1103): the k-space time drops from 1.63 s (CPU `ewald`) to 0.14 s
(`ewald/gpu`), about 12x, and is ~2.6x faster than `pppm/gpu`'s k-space at this
size, with total loop times essentially tied with `pppm/gpu`.

CUDA and HIP support is inherited from the backend-agnostic kernel source but
has only been tested with OpenCL; it should be verified on NVIDIA/AMD hardware.
A half-range `cs`/`sn` layout and an on-device triclinic kernel remain as
possible future optimizations.

Compilation with OpenCL and HIP and successful execution has been confirmed
on gfx1103 architecture.

**Post Submission Checklist**

- [ ] The feature or features in this pull request is complete
- [ ] Licensing information is complete
- [ ] Corresponding author information is complete
- [ ] The source code follows the LAMMPS formatting guidelines
- [ ] Suitable new documentation files and/or updates to the existing docs are included
- [ ] The added/updated documentation is integrated and tested with the documentation build system
- [ ] The feature has been verified to work with the CMake based build system

**Further Information, Files, and Links**

None.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f2f2286a153f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.