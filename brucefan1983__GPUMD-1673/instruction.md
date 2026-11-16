# Issue

**Title:** Apply PIMD Berendsen barostat scaling only to centroids

## Description

#  **Summary** (Briefly summarize the purpose of this pull request)

The barostat scaling should be applied only to the centroid coordinates, consistent with the NPT-PIMD formulation of Ceriotti et al. （https://doi.org/10.1016/j.cpc.2013.10.027, based on the constant-pressure path-integral framework of Martyna et al. （https://doi.org/10.1063/1.478193）

Tests show that the old and new implementations produce essentially identical macroscopic results for the Berendsen barostat, even though they treat the internal ring-polymer modes differently. 


# **Modification** (Describe the main changes made in this pull request)

- Apply isotropic, orthogonal, and triclinic barostat transformations to
  the ring-polymer centroids.
- Translate all beads of each atom by the same centroid displacement,
  preserving the internal bead-to-centroid coordinates.
- Leave the pressure estimator, PBC handling, and input syntax unchanged.

#  **Licensing** (Please read and confirm before submitting this pull request)

By submitting this pull request, I agree that my contribution will be included in GPUMD and redistributed under the existing GPUMD license. Newly added source files follow the existing GPUMD license-header style when applicable. No external source code with incompatible licensing has been copied into this pull request.

# **Validation** (Describe how this pull request was tested)

- With `-DDEBUG`, the old and new executables give identical NVT bead coordinates for both cubic and triclinic cells.
- One-step isotropic, orthogonal, and triclinic NPT tests preserve the internal bead-to-centroid coordinates within 7.5e-9 A.
- An 8000-atom Si system with 8 beads was run for 10 ps. The pressure converged to 0 GPa, and the old and new implementations produced essentially identical mean pressure and volume.

# **Others**

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5881dc52f17d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.