# Issue

**Title:** Enable protein mutations involving nonstandard protonation state aas

## Description

## Description
Enable protein mutation transformations involving nonstandard amino acids, specifically: ASH, GLH, HID, HIE, HIP, LYN
- LYN is deprotonated LYS (neutral)
- ASH is protonated ASP (neutral)
- GLH is protonated GLU (neutral)
- HIE is histidine protonated at N-epsilon (neutral)
- HID is histidine protonated at N-delta (neutral)
- HIP is histidine protonated at both N-delta and N-epsilon (+1 charge)

Note that if HIS is specified, we use the HIE protonation state.

<!-- Describe your changes in detail. -->
Changes:
- Enable `perses.app.relative_point_mutation_setup.PointMutationExecutor` to take in an optional argument that specifies a nonstandard amino acid name for the old residue. 
    - If there are nonstandard residues, the input PDBs should contain the standard residue name but the atoms/positions should correspond to the nonstandard residue. 
    - The `PointMutationExecutor` will generate the old and new systems with the standard amino acid name (as OpenMM requires standard amino acid names even for nonstandard amino acids) and then it will modify the old residue name to the nonstandard name specified. 
    - From there on, the nonstandard name will be used in subsequent calls to generate a topology proposal, geometry proposals, and hybrid topology factories. 
    - Note: If the user wants to mutate to a nonstandard amino acid, simply supply the nonstandard amino acid name as the `proposed_residue` argument, per normal (nothing changed here)
- Expanded `ring_amino_acids` in `relative_point_mutation_setup.py` to include HID, HIE, HIP
- Added PDB templates (generated in PyMOL) for the aforementioned nonnstandard amino acids in `perses/data/aminio_acid_templates` -- these are used to generate oemols of the old and new residues
- Expand `perses.rjmc.topology_proposal.PolymerProposalEngine._aminos` to include `['ASH', 'GLH', 'HID', 'HIE', 'HIP', 'LYN']`. Changed `PolymerProposalEngine._positive_aminos` to be `['ARG', 'HIP', 'LYS']`
- Deleted lines in `topology_proposal.py` that force HID/HIE/HIP to HIS  -- previously we didn't have HID/HIE/HIP PDB templates, so we had to use the HIS template, but we don't need this behavior anymore
- Deleted unused attribute `self._aminos_3letter_to_1letter_map` from `topology_proposal.py`
- Expanded `test_topology_proposal.py`'s `test_mutate_from_alanine` to include the aforementioned nonstandard amino acids 
- Removed the automatic conversion of HID/HIE to HIS in `test_topology_proposal.py`'s `test_mutate_from_every_amino_to_every_other`

## Motivation and context

<!--- Why is this change required? What problem does it solve? -->

<!-- Replace ??? with the issue number that this pull request resolves. -->


## How has this been tested?

<!--- Please describe in detail how you tested your changes. -->
Expanded `test_topology_proposal.py`'s `test_mutate_from_alanine` to include the aforementioned nonstandard amino acids.

## Change log

<!-- Propose a change log entry. -->
<!-- Examples here https://github.com/choderalab/perses/blob/master/docs/changelog.rst -->
```

```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `82dd766968f2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.