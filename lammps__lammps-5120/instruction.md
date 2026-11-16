# Issue

**Title:** [BUGFIX] Correct harmonic restoring force in fix spring/chunk

## Description

**Summary**

Correct the restoring force applied by `fix spring/chunk` so that it is the
negative gradient of the documented and reported harmonic potential.

The previous implementation normalized the COM displacement by its magnitude,
which made the total force on every displaced chunk have constant magnitude
`K`. The corrected force is linear in the COM displacement.

**Related Issue(s)**

.

**Author(s)**

Carlos Monago, UNED

Corresponding author: csmonago@fisfun.uned.es

**Licensing**

By submitting this pull request, I agree, that my contribution will be included
in LAMMPS and redistributed under either the GNU General Public License version
2 (GPL v2) or the GNU Lesser General Public License version 2.1 (LGPL v2.1).

**Artificial Intelligence (AI) Tools Usage**

OpenAI Codex was used to analyze the force/energy inconsistency and draft the
source and regression-test changes. The contributor reviewed the analysis,
inspected the changes, and validated the behavior with a locally compiled
LAMMPS binary.

**Backward Compatibility**

Input syntax and documented behavior are unchanged. Numerical trajectories that
use `fix spring/chunk` will change because this corrects the force from a
constant-magnitude radial force to the documented harmonic force. This is an
intentional bug fix; simulations that relied on the previous undocumented
behavior will not reproduce their old trajectories.

**Detailed Description**

For COM displacement `d = Rm - R0m`, the fix reports

```text
U = 0.5 * K * |d|^2.
```

The old code applied

```text
Fchunk = -K * d/|d|,
```

while the consistent harmonic force is

```text
Fchunk = -K * d.
```

This pull request removes the displacement normalization from the three force
components. It also computes the energy from the unclamped squared displacement,
so the spring energy is exactly zero at the reference COM.

The regression test uses a one-atom, one-chunk system with `K=2`. At COM
displacements `0.25` and `0.50`, it checks the exact force/energy pairs
`(-0.5, 0.0625)` and `(-1.0, 0.25)`. It also checks the applied force against a
central finite difference of the fix energy. The existing
`fix-timestep-spring_chunk` reference was regenerated because it recorded the
buggy trajectory.

Verification performed:

- rebuilt the CMake `lmp` target successfully
- minimal one-atom reproducer gives `(Fx,U) = (0,0)`, `(-0.5,0.0625)`, and
  `(-1.0,0.25)` at displacements `0`, `0.25`, and `0.50`
- `ctest -R '^(FixSpringChunk|FixTimestep:spring_chunk)$'` passes both tests;
  the generic fix test includes Verlet and r-RESPA coverage
- LAMMPS `check-whitespace` and `check-permissions` pass
- `git diff --check` passes

No documentation behavior change is needed because the existing manual already
describes the harmonic force and quadratic energy implemented by this fix.

**Post Submission Checklist**

- [x] The bug fix is complete
- [x] Licensing information is complete
- [x] Corresponding author information is complete
- [x] The source code follows the LAMMPS formatting guidelines
- [x] The feature has been verified to work with the CMake based build system
- [x] Suitable tests have been added to the unittest tree

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2cb47c3845de`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.