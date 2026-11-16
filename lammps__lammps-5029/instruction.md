# Issue

**Title:** Fix Kokkos eam/fs/kk full-neighbor pressure bug

## Description

**Summary**

Fix a Kokkos `eam/fs/kk` full-neighbor pressure bug by removing a duplicate invocation in the no-reduce `PairEAMFSKokkos::operator()(TagPairEAMFSKernelAB<EFLAG>, const int &ii)` overload.

For `neigh full` runs, the no-reduce overload was calling the same KernelAB implementation twice. On non-thermo steps where `eflag == 0`, this duplicated the electron-density accumulation into `d_rho[i]`, causing incorrect pressure and volume evolution for `pair_style eam/fs` with Kokkos full neighbor lists.

I first noticed this while running LAMMPS on a 1000-V100 GPU cluster named SAI, which was built by @Entropy-Enthalpy . The LAMMPS/Kokkos GPU acceleration path is extremely valuable for this kind of workload, so I tried to reduce the observation to a small reproducer: the CPU/half-neighbor path behaved normally, while the Kokkos CUDA full-neighbor path produced a rapidly diverging negative pressure. Thank you for maintaining such a powerful and widely useful codebase; I hope this small fix is helpful.

**Related Issue(s)**

No existing GitHub issue is referenced.

**Author(s)**

Jiacheng Xu,Westlake University.
Contact:13862180016@163.com

**Licensing**

By submitting this pull request, I agree, that my contribution will be included in LAMMPS and redistributed under either the GNU General Public License version 2 (GPL v2) or the GNU Lesser General Public License version 2.1 (LGPL v2.1).

**Artificial Intelligence (AI) Tools Usage**

By submitting this pull request, I confirm that I did NOT use any AI tools to generate all or parts of the code and modifications in this pull request.

**Backward Compatibility**

This change does not alter input syntax, public APIs, data formats, or documented user-facing behavior. It corrects the Kokkos `eam/fs/kk` full-neighbor execution path so that it matches the existing half-neighbor reference behavior.

**Implementation Notes**


The affected no-reduce overload previously invoked the reduced KernelAB implementation twice:

```cpp
void PairEAMFSKokkos<DeviceType>::operator()(TagPairEAMFSKernelAB<EFLAG>, const int &ii) const {
  EV_FLOAT ev;  this->template operator()<EFLAG>(TagPairEAMFSKernelAB<EFLAG>(), ii, ev);
  this->template operator()<EFLAG>(TagPairEAMFSKernelAB<EFLAG>(), ii, ev);
}
```

This pull request removes the second call:

```cpp
void PairEAMFSKokkos<DeviceType>::operator()(TagPairEAMFSKernelAB<EFLAG>, const int &ii) const {
  EV_FLOAT ev;  this->template operator()<EFLAG>(TagPairEAMFSKernelAB<EFLAG>(), ii, ev);
}
```

Build configuration:

- LAMMPS: `30 Mar 2026 - Development`, Git `develop / b3da81c`
- Compiler: GNU C++ 13.3.0
- MPI: Open MPI 5.0.8
- CUDA: 12.4.1
- Kokkos: 5.1.99
- Packages: `KOKKOS MANYBODY`
- Kokkos backends: `CUDA OpenMP Serial`
- Architecture flags: `Kokkos_ARCH_VOLTA70=ON`, `Kokkos_ARCH_ZEN3=ON`

The test case is a 1024-atom BCC Fe `pair_style eam/fs` NPT run using `Fe_mm.eam.fs`, run for 5000 steps on one V100 GPU.

Run matrix:

| Case | Binary | Kokkos attributes | Step 5000 Press | Step 5000 Volume |
| --- | --- | --- | ---: | ---: |
| unpatched full | unpatched `b3da81c` | `full, newton off, kokkos_device` | `-64383.726` | `12519.281` |
| patched full | one-line patched `b3da81c` | `full, newton off, kokkos_device` | `-636.05337` | `11969.031` |
| patched half reference | one-line patched `b3da81c` | `half, newton on, kokkos_device` | `-636.05337` | `11969.031` |

The patched full-neighbor run matches the patched half-neighbor reference exactly at all thermo output points in this test.

**Post Submission Checklist**

- [x] The feature or features in this pull request is complete
- [x] Licensing information is complete
- [x] Corresponding author information is complete
- [x] The source code follows the LAMMPS formatting guidelines
- [ ] Suitable new documentation files and/or updates to the existing docs are included
- [ ] The added/updated documentation is integrated and tested with the documentation build system
- [ ] The feature has been verified to work with the conventional build system
- [x] The feature has been verified to work with the CMake based build system
- [ ] Suitable tests have been added to the unittest tree.
- [ ] A package specific README file has been included or updated
- [x] One or more example input decks are included

**Further Information, Files, and Links**

- the three LAMMPS logs,
- the Slurm output,
- the small Fe EAM/FS input deck,
- the Slurm matrix script.

[eamfs_pr_artifacts.zip](https://github.com/user-attachments/files/28802497/eamfs_pr_artifacts.zip)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b3da81cd3a32`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.