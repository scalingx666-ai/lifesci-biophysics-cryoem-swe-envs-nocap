# Issue

**Title:** Optimize the calculation of fit gradients and update the documentation (fix #560)

## Description

This PR avoids the expensive `position_derivative_inner` by projecting the forces on the rotated frame to 9 rotation matrix elements at first, and then projecting the forces to the quaternion. This PR also completes  and updates the time complexity of fit gradients in the user manual.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `dc3246de17c0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.