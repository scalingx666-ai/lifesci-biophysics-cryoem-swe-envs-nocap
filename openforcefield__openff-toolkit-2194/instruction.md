# Issue

**Title:** Add support for N-terminal, protonated, disulfide-bonded cysteine

## Description

- [x] Tag issue being addressed
- [x] Add [tests](https://github.com/openforcefield/openff-toolkit/tree/main/openff/toolkit/_tests)
- [x] Update docstrings/[documentation](https://github.com/openforcefield/openff-toolkit/tree/main/docs), if applicable **n/a**
- [x] [Lint](https://docs.openforcefield.org/projects/toolkit/en/stable/users/developing.html#style-guide) codebase
- [x] Update [changelog](https://github.com/openforcefield/openff-toolkit/blob/main/docs/releasehistory.md)

.

This PR adds support for loading N-terminal, protonated, disulfide-bonded cysteine residues from PDB files.

Specifically, this covers a `CYS` residue with:

- N-terminal `NH3+`
- neutral degree-2 side-chain `SG`
- no `HG`
- external `SG-SG` disulfide connectivity

Changes made:

- Adds missing CYS substructures in `utilities/make_substructure_dict/_cif_to_substructure_dict.py`.
- Regenerates:
  - `aa_residues_substructures_with_caps.json`
  - `aa_residues_substructures_explicit_bond_orders_with_caps.json`
  - `aa_residues_substructures_explicit_bond_orders_with_caps_explicit_connectivity.json`
- Adds a minimal regression PDB fixture for this chemistry.
- Adds a regression test covering ingestion with `Molecule.from_polymer_pdb`.
- Updates `docs/releasehistory.md`.
- Updates the substructure-generator CIF download URL from `ftp.wwpdb.org` to `files.wwpdb.org`, since `ftp.wwpdb.org` did not resolve locally but the same wwPDB file was available from `files.wwpdb.org`.

Validation run locally:

```bash
mamba run -n openff-toolkit-ncy pytest openff/toolkit/_tests/test_molecule.py -k "cys_disulfide or mainchain_cyx_dipeptide or mainchain_cys_dipeptide or mainchain_cym_dipeptide"
```
Result:
4 passed
Additional checks:
The regenerated explicit-connectivity SMARTS library validates successfully.
The motivating protein, PDB=4CHA, now loads with Topology.from_pdb, giving 3519 atoms and 1 molecule.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8e39c98fb2a1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.