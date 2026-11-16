# Issue

**Title:** Fix ELCIC nonneutral for CPU, disable for GPU

## Description

, disables combination described in #5408

This now finally re-enables ELCIC for non-neutral systems. It turns out, that also the not yet disabled metallic case was also affected by #3001.

<img width="6300" height="2850" alt="fig1_impact" src="https://github.com/user-attachments/assets/4b14c056-afe1-4f94-aeb1-1caab4406f3b" />

- For neutral systems, the dipole correction is translational invariant. However, for non-neutral systems it's not. So one part was to measure the dipole moment with respect to the symmetry-plane of the system. This caused a constant force towards one of the plates (depending on a sign), which is now corrected.
- The force was not the gradient of the energy. Due to a mistake in the sign of the energy and a mess between `lz` with and without gap for the corrections, we had a jump in our energy.
- The background_energy-correction counted the energy correction twice.
- The analytic ImageSum used `box_length` instead of `box_h`.
- I've enabled non-neutral with non-metallic boundaries as well, as I don't see why this was forbidden. To the best of my knowledge it works exactly the same as the metallic case.
- Fixed an interface issue, where `const_pot` was silently ignored by the code.
- Disabled P3M-GPU with dielectric ELC, due to #5408

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7766bfc2d221`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.