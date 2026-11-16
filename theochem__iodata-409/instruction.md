# Issue

**Title:** Add functions to map between basis indices and labels

## Description

.

`MolecularBasis.conventions` already holds the ordered labels for each `(angmom, kind)` pair, so both directions asked for in the issue follow from a single walk over the shells and their generalized contractions. Nothing new is invented about naming: the labels come from the basis object itself.

Adds:

- `BasisFunction`, a namedtuple identifying one function by `index`, `ishell`, `icon`, `icenter`, `angmom`, `kind`, `label` and `sign`
- `MolecularBasis.iter_basis_functions()`, walking them in basis order
- `MolecularBasis.get_basis_function(index)`, index to label
- `MolecularBasis.find_basis_index(label, icenter, occurrence)`, label to index

Following @PaulWAyers' comment, the sign convention is tracked rather than folded into the label. A convention entry such as `-c3` gives label `c3` with `sign = -1`, so `find_basis_index("c3")` finds it and the caller can see that it is flipped. None of the bundled fixtures use a sign-flipped convention, so that test builds ORCA's pure f convention straight from the `MolecularBasis` docstring.

Also per that comment I have left this as one PR rather than splitting it, since the size was not a blocker.

Testing: `pytest iodata/test -m "not slow"` gives 524 passed, up from 517. Checked separately against `water_sto3g_hf_g03.fchk`, `h2o_sto3g.wfn`, `ch3_rohf_sto3g_g03.fchk`, `monosilicic_acid_hf_lan.fchk` and `nh3_molden_pure.molden`: the number of walked functions equals `nbasis` in every case and matches the first dimension of the MO coefficients, and index to label to index round trips hold for each. `ruff check` and `ruff format --check` are clean.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4fa631838a44`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.