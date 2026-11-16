# Issue

**Title:** force_constants: third order, fitted with the second

## Description

Step (d) of the lattice dynamics plan: third order force constants, after Fable's design review. I reran its prototype; its numbers and these agree. It builds on #63 (bond stiffness), whose commits this branch holds until that one merges.

## What it adds

- `force_constants(cell, frames, *, cutoff=None, cutoff_third=None, symmetry=None)`.
  - `cutoff_third` (Å) turns on one joint least squares of second and third order from the same frames: random patterns by plus and minus, as `displaced_supercells(method="random")` makes them.
  - The cutoff limit is the same as for `cutoff`.
- `ForceConstants` gains:
  - `third`, dense (n, nS, nS, 3, 3, 3): phono3py's compact layout with MDAPY's atom order. It is accepted as the fourth constructor argument and refused above 2 GB.
  - `cutoff_third` and `parameters_third`.
- `write` still writes the second order only. No installed package reads a third-order text format, so a writer could only be tested against our own reader.

## Design

- **Triplets:** two pairs of the pair table with the same first atom in the cell, whose second atoms are also within the cutoff of each other.
- **Symmetry:**
  - every operation of the crystal and the six orders of the three atoms;
  - group-averaged projectors on the 27 components, as for second order;
  - the sum rule over the third atom for one pair of each pair orbit, by null space.
- **Design matrix:** the product of the displacements of the two atoms, contracted by matrix products per orbit and first atom.
  - Fable's heaviest case (fcc cubic 4×4×4, 34108 triplets, K₃ = 810) takes 0.8 s at a 1 GB peak. Fable's NumPy prototype took 7.6 s at 2 GB, so no C++ kernel is needed.
- **Joint solve:** columns are scaled because the two orders differ as u and u², and the rank check tells that third order needs random displacements.
- **Refactor:** the pair lookup, the pair images, the frame matching and the translation table become shared helpers. Earlier results are unchanged; the full suite passes (3793).

## How the results are shown to be right

- **Closed form, nearest-neighbour Morse:** the forces of its Taylor expansion to third order give back both orders exactly, below 1e-12.
  - Cases: fcc, hcp (on-site third order nonzero) and diamond, plus fcc 3×3×3 with a cutoff above a third of the supercell.
  - `third` is symmetric in its last two atoms and sums to zero over the third.
- **The potential's own forces:** the errors of both orders fall as h², with a ratio between 3.5 and 4.5 for h = 0.02 and 0.01.
- **symfc 1.7.3 live** (`orders=[2, 3]`, `cutoff={3: r}` or `{2: r2, 3: r3}`): second and third order agree to 1e-12 for fcc primitive and cubic, hcp and L1₂, on the torus and within a cutoff. Cutoffs are between shells and below L/3, where symfc's per-distance minimum image equals ours.
- **The second order of the joint fit** equals that of a fit of second order alone on ± patterns, to 1e-12.
- **Errors:** finite moves of single atoms are refused for third order, and a cutoff_third that is too long or memory over 2 GB is refused. Coverage of the module is 99.8 %.

## The page

The Force constants page has a "Third order" part with an example. For copper, `error` falls from 4 % with second order alone to 0.7 % with nearest-neighbour third order and 0.2 % within 5 Å. The page also covers the amplitude (errors as h²; 0.01 Å without noise, about 0.03 Å for DFT) and why patterns must be random and come in ± pairs. The definition and the errors are extended too.

Next: (b) group velocities, which the RTA thermal conductivity (e) needs, then (e) against phono3py.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9249aa74faaa`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.