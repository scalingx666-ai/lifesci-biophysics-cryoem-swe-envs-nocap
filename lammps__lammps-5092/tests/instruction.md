# Issue

**Title:** New chunk-based compute for coarse graining granular fields

## Description

**Summary**

A new compute for coarse graining continuum fields has been added to the GRANULAR package based on the formulation by Goldhirsch. The implementation is quite similar to fix ave/chunk except the compute is uses a smoothing kernel to weight contributions from atoms and it does not perform the actual averaging (needs to be handed off to fix ave/time). Thus, the cutoff of the smoothing kernel can be larger than a chunk size such that atoms outside of a chunk can contribute to that chunk's value (when the kernel cutoff is > than the chunk size). The size/position of chunks therefore corresponds more to the position/resolution of the resulting coarse-grained field.

The compute only works with bin-based chunks (e.g. not spherical, cylindrical...).

Lastly, fixed a few uninitialized variables in granular model. PR #5219 had an alternate initialization of `dq`, but either works.

**Author(s)**

Joel Clemmer (SNL)

**Licensing**

By submitting this pull request, I agree, that my contribution will be included in LAMMPS and redistributed under either the GNU General Public License version 2 (GPL v2) or the GNU Lesser General Public License version 2.1 (LGPL v2.1).

**Artificial Intelligence (AI) Tools Usage**

Github copilot was used to scan code for possible bugs/errors and draft the port from the fix to compute implementation (a lot of replacing).

**Backward Compatibility**

NA

**Implementation Notes**

NA

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6aad7d7c2912`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.