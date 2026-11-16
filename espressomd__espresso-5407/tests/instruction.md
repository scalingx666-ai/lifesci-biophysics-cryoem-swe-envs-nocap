# Issue

**Title:** Dipolar P3M dipole field tracking

## Description

This PR adds dipole-field tracking support to the Kokkos implementation of `DipolarP3M`.

When `DIPOLE_FIELD_TRACKING` is enabled, DP3M now calculates and stores the total magnetic field acting on each particle as part of the existing force-and-torque calculation. This avoids a separate field-calculation pass and keeps the field consistent with the torque used by the integrator.

The implementation includes all DP3M contributions:

- Real-space pair contribution
- Reciprocal-space mesh contribution
- Dipolar surface term
- Local and ghost-particle contributions across MPI ranks

## Implementation

A Kokkos `ScatterView` is added for the local dipole field, following the existing force and torque accumulation model.

During force calculation:

1. The dipole-field buffers are reset together with the force and torque buffers.
2. The DP3M real-space kernel returns the field acting on both particles in each pair.
3. The reciprocal-space torque interpolation also accumulates the corresponding field.
4. The surface-term field is added directly to each particle.
5. The Kokkos scatter buffer is reduced into `Particle::dip_fld()`.
6. Dipole fields accumulated on ghost particles are reduced back to their owning particles.

Consequently, field tracking uses the same real-space and reciprocal-space evaluations as the force and torque calculation and does not require an additional DP3M evaluation. The implementation supports both distributed-memory and shared-memory parallelism:
- MPI ghost communication now supports dipole-field reduction.
- Kokkos accumulation uses a dedicated `ScatterView`, consistent with the current force and torque implementation.
- The field is reset and reduced according to the existing force-calculation lifecycle.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `36c1560db6a4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.