# Issue

**Title:** kokkos'ify pressure

## Description

- **Performance work: short range forces**
- **Only call force funciotns, if force active**
- **diagnostics**
- **avoid std::function in for_each_particle()**
- **Missing file**
- **Revert ELC short range changes**
- **Aoivd matrix ops in langevin thermostat if particle is isotropic**
- **Short range opt**
- **Add design spec: pressure/stress tensor Kokkos/Cabana conversion**
- **Add implementation plan: pressure/stress tensor Kokkos/Cabana conversion**
- **pressure: add position-based overloads of bonded virial helpers**
- **forces: add director-based calc_non_central_force overload for pressure Cabana kernel**
- **pressure: add PressureBinLayout, PressureKernel, reduce_cabana_pressure**
- **pressure: add Kokkos bond pressure kernels (pair, angle, null dihedral)**
- **pressure: replace short_range_loop with Cabana pair loop; integrate DPD virial**
- **pressure: fix DPD virial sign in PressureKernel**
- **pressure: fix DPD virial guard — add dpd_active() check and move inside max_cut guard**
- **pressure: remove dead calc_bonded_pressure_tensor dispatcher**

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `dfa7367899a7`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.