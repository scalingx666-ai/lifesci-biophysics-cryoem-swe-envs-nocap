# Issue

**Title:** thermal_conductivity: linear tetrahedra by default

## Description

Part of step (e): the tetrahedron method, after Fable's prototype. I verified its weights against the NumPy original, and the full results against phono3py. It builds on #69 (C++ kernel); this branch holds its commits until #68 and #69 merge.

## What it adds

`thermal_conductivity(..., sigma=None)`: by default, the delta functions of energy conservation are integrated over linear tetrahedra. Calls that give `sigma` still use the Gaussian.
- **Tetrahedra:** the 24 that share each mesh point, cut along the shortest main diagonal. The function integrated is ω′(q′+r) ± ω″(q″−r), with corner frequencies clipped at 0 and the level the raw ω(q).
- **Weights:** Blöchl's "I" weights of the point. A level on a corner belongs to the range above it, and corners within 1e-10 count as one.
- **Symmetry:** the tetrahedra of every rotation of the crystal are used, as phono3py's `symmetrize_tetrahedra`, so γ does not depend on which member of an orbit is the irreducible point, and κ keeps the crystal's symmetry exactly.
- **Degenerate modes:** the weights are averaged over modes of one frequency at q′ and q″, as phono3py's `average_degenerate_weights`. Fable found that phono3py's plain default is not well defined: switching the LAPACK triangle moves it by 2e-3 to 4e-3, because the split of |Φ|² among degenerate modes depends on the eigenvectors.
- **Speed:** `_core.analysis.tetrahedron_triplet_weights` computes the weights in C++, parallel over q′, 44 times faster than NumPy. The result goes into the scattering kernel of #69.

## How the results are shown to be right

- **phono3py 4.8.2 live,** with `symmetrize_tetrahedra=True, average_degenerate_weights=True`: κ and every γ agree to 1e-5 (actual about 3e-7) for fcc 9³, hcp 6×6×4 and L1₂ 5³, at 100, 300 and 600 K. The Gaussian comparisons stay.
- **Weights alone:** C++ equals the NumPy prototype to 3e-16 on random corners with many ties and a level on a corner. Fable's prototype matched phono3py's weights to 1e-13.
- **Closed forms:**
  - the weight of each corner of one tetrahedron integrates to one over the levels, whichever corner is the point and with equal corners;
  - summed over the mesh, the weights of the phonon frequencies equal the tetrahedron density of states of `Phonons.dos` (a separate implementation) to 1e-10.
- **Symmetry:** cubic κ is one number to 1e-10.
- Coverage of the module is 100 %. GCC 15 with -Werror and the TSan compile pass, and so does the full suite.

## The page

The page now has:
- a part on the conservation of energy, explaining why the tetrahedra are the default: for copper at 300 K a 0.1 THz Gaussian gives 222 W/(m K) on a mesh of 5 where the tetrahedra give 8.6, and 8.1 against 7.4 on a mesh of 7;
- updated convergence numbers: copper at 300 K is 7.72, 7.89 and 7.96 W/(m K) on meshes of 11, 15 and 19;
- timings: 4 s on a mesh of 11, 22 s on 15;
- the picture rerendered with the default.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b11c221a06ad`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.