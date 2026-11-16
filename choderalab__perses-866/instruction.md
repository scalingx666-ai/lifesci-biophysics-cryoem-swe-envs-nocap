# Issue

**Title:** Fix endstate validation handling in PointMutationExecutor

## Description

## Description

The default behavior of `PointMutationExecutor` in Perses 0.9.2 leads to a `NameError` since `forward_htf` is not defined. This PR aims to fix the endstate validation handling.

## Motivation and context

<!--- Why is this change required? What problem does it solve? -->
The defaults in `PointMutationExecutor` lead to a `NameError`.

<!-- Replace ??? with the issue number that this pull request resolves. -->
 

## How has this been tested?

I tested the changes with an ABL1:dasatinib system ([protein](https://github.com/openkinome/study-abl-resistance/blob/update_modeling/data/tki_complexes/kinoml_OEKLIFSKinaseHybridDockingFeaturizer_ABL1_2gqg_chainA_altlocA_dasatinib_protein.pdb), [ligand](https://github.com/openkinome/study-abl-resistance/blob/update_modeling/data/tki_complexes/kinoml_OEKLIFSKinaseHybridDockingFeaturizer_ABL1_2gqg_chainA_altlocA_dasatinib_ligand.sdf)):

```
solvent_delivery = PointMutationExecutor(
    protein_path, 
    '1', # First and only protein chain 
    str(resid), 
    mutate_to,
    ligand_input=ligand_path,
    ionic_strength=0.15*unit.molar,
)
```

## Change log

<!-- Propose a change log entry. -->
<!-- Examples here https://github.com/choderalab/perses/blob/master/docs/changelog.rst -->
```
Adapt endstate validation handling in PointMutationExecuter to be only done for the vanilla HybridTopologyFactory. 
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `410ca4f82fdf`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.