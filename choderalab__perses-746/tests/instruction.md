# Issue

**Title:** Use a list of oemols for PointMutationExecutor input instead of files

## Description

This PR adapts the input of `PointMutationExecutor` to allow for a list of oemol objects to be used instead. Both @schallerdavid and I were having difficulty parsing ligand `.sdf` [files](https://github.com/openkinome/study-ntrk-resistance/tree/perses_sims/notebooks/kinoml_modeling/data) containing a nitrogen that was perceived as needing to be tetrahedral at [this line](https://github.com/choderalab/perses/blob/master/perses/utils/openeye.py#L366) in `perses.utils.openeye`. This PR allows the user to bypass internal oemol preparation. 

Changed:
- `PointMutationExecutor` now takes inputs: `.sdf`, `.pdb`, `[oemol]`.
- `ligand_file` renamed to `ligand_input` to be more general.

An example script of how to prepare the `[oemol]` object is below, input names changed from my use case:

```python
from perses.app.relative_point_mutation_setup import PointMutationExecutor
from perses.utils.openeye import generate_unique_atom_names
import oechem
from simtk import unit

ifs = oechem.oemolistream()
ifs.open('./ligand.sdf')
mol_list = [oechem.OEMol(mol) for mol in ifs.GetOEMols()]
mol = mol_list[0]

if len([atom.GetName() for atom in mol.GetAtoms()]) > len(set([atom.GetName() for atom in mol.GetAtoms()])):
        mol = generate_unique_atom_names(mol)
oechem.OEAssignAromaticFlags(mol, oechem.OEAroModelOpenEye)
oechem.OEAssignHybridization(mol)
oechem.OEAddExplicitHydrogens(mol)
oechem.OEPerceiveChiral(mol)

mol_list = [mol]
protein_file = './protein.pdb'

x = PointMutationExecutor(
protein_file,
'1',
'510', # an example residue number
'VAL', # an example mutation
ligand_input=mol_list,
ionic_strength=0.15 * unit.molar,
small_molecule_forcefields='openff-1.2.0'
)
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f640bf82d752`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.