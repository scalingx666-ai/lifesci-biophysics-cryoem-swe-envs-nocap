# Issue

**Title:** force_constants: a cutoff, with every operation of the crystal

## Description

The cutoff regime of the force constants plan, the base for BondStiffness and for third order. It builds on #61 (and #60); this branch holds their commits until they merge.

## What it adds

- `force_constants(cell, frames, *, cutoff=None, symmetry=None)`: with `cutoff` in Å, the fit holds only the pairs of the crystal no farther apart than it, zero beyond. Each pair is under **every** operation of the crystal, also in supercells of less symmetry (4×4×3 of fcc), together with the exchange of the two atoms and the sum rule.
- `ForceConstants.cutoff` holds the value used, or `None`.
- Limits on the cutoff:
  - The cutoff is at most half the shortest vector of the supercell less 1e-3 Å, so that each pair is its own shortest image; the error message gives the limit.
  - A cutoff typed at the distance of a shell holds the whole shell (pairs within 1e-4 Å beyond it count).
- `Phonons` and the dense `(n, nS, 3, 3)` `second` are unchanged. Each pair sits at its atom of the supercell, and its shortest image is the pair itself, so the frequencies are those of a direct sum over the pair vectors.
- Implementation: the pair table of the infinite crystal `(a, b, R)` is new. The orbits, projectors, sum rule (null space) and dense layout are shared with the torus fit.

## How the results are shown to be right

- **Springs in closed form** (along a bond φ″ = k, across it φ′/r, in tension), with forces exactly of them: they come back to 1e-12 eV/Å².
  - Cases: fcc 4×4×4, fcc 4×4×3 (less symmetry), a skewed supercell of 65 cells, hcp 4×4×3, diamond 3×3×3, and a triangular sheet.
  - Each with and without symmetry.
  - The frequencies equal a direct sum over the pair vectors to 1e-10 THz.
- **symfc 1.7.3 live, with `cutoff={2: r}`:** on the same random displacements and EAM forces, the results agree to 1e-12 for fcc primitive and cubic cells, bcc, hcp and L1₂. These are supercells that keep every operation, with cutoffs between shells, because symfc leaves out a shell at the cutoff.
- **Supercell of less symmetry:** fcc 4×4×3 at 2.8 Å has 3 free parameters, against symfc's 20 from the kept operations only. The frequencies are invariant under every rotation of the crystal to 1e-10.
- **Shell at the cutoff:** a cutoff typed at the nearest-neighbour distance holds all 12 neighbours.
- **Convergence:** the error falls as the cutoff grows, toward the torus fit.
- **Errors:** a cutoff that is not positive or is too long is refused.
- Module coverage is 99.8 %. The full suite passes.

The page states how far the frequencies of copper (`EAM.zhou`, 6×6×6) move with the cutoff: 0.3 THz at 2.6 Å, 0.11 at 4.6, 0.04 at 5.8, 0.005 at 7.0. `error` stops falling once anharmonic forces dominate it (from 4.6 Å here), so the page advises comparing the frequencies of two cutoffs.

Left out on purpose: low-symmetry cells such as SQS in large supercells, which would need a sparse iterative solver and stay with `phonons`/`phonons_from_frames`; any public cluster form; order 3.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a352e7e5cbf7`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.