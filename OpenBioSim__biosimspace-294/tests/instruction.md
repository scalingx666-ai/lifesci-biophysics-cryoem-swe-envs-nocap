# Issue

**Title:** Add Protein FEP Functionality

## Description

This PR introduces protein FEP functionality to BioSimSpace and allows for creating of hybrid protein/peptide systems with single or multiple simultaneous modifications, which can include point mutations to canonical or non-canonical amino acids, as well as covalent modifications.

The PR is essentially based around region of interest (ROI) idea, and code wise does the following:

- Moves the `merge` function from [Exscientia sandpit](https://github.com/OpenBioSim/biosimspace/blob/devel/python/BioSimSpace/Sandpit/Exscientia/Align/_merge.py) out of the sandpit into the standard merge code location. I have tested this code with ethane to methanol perturbation, and it gives the same exact perturbation as the non-sandpit merge code, (I looked at gromacs .top files specifically) although it is probably a good idea to test this with more complex ligand perturbations.
- This merge code is then modified to remove the need to provide ROI atom indices as inputs (this was a requirement in original Exscientia's implementation). Instead these are reconstructed inside the merge function which simplifies the overall workflow for the user, as now they only need to pass the ROI residue indices. The merge function is then modified to allow for multiple simultaneous merges to be performed between two molecules. This was tested with MDM2 T16G-I19G double mutant system and behaves as intended. Note that these modifications are only invoked when using ROI code only, it does not affect the standard merge code.
- The `roiMatch` function is then added to the Align module. This function rapidly computes mappings between two large input molecules based on the idea of region of interest. It supports standard BioSimSpace.Align.matchAtoms as well as [Kartograf](https://github.com/OpenFreeEnergy/kartograf) as mapping backends. Therefore an internal `_kartograf_map` function is also added to the Align module. In my experience kartograf in the protein FEP context is really only needed for niche cases such when trying to compute a mapping between two enantiomers of two covalently modified residues, so it's not critical to have it as a mapping backend for most use cases. The `matchAtoms` function is **also modified in order to expose some of more lower-level rdKit mapping functionality** which I found useful sometimes when trying to force a specific mapping for covalently modified residues. It does not affect the default behaviour of the mapping function.
- The `roiAlign` function is also added to the Align module. This function aligns selected residues from molecule0 to molecule1, which allows us to drop the assumption that two input proteins need to be in the same conformation in order to create a merged protein. In most cases this assumption is satisfied (if you just create a carbon copy of the wild-type protein and mutate 1 or more residues), however with this function we can execute more complex workflows such as non-equilibrium switching protein FEP (where you would generate wild-type and mutant trajectories separately without alchemical code, and then start to create hybrid snapshots that now have to deal with two input protein structures which are not fully aligned). This function can either use `rmsdAlign` or `flexAlign` functions internally, depending on how precise alignment is needed. In my experience `rmsdAlign` works perfectly after some minimisation.

Overall, this means that a hybrid proteins can be created with just few lines of code that are nearly identical to the normal input creation for FEP workflow:

```python
import BioSimSpace as BSS
mdm2_wt = BSS.IO.readMolecules(f"mdm2_wt.pdb")[0]
mdm2_mut = BSS.IO.readMolecules(f"mdm2_v14g.pdb")[0]

mdm2_wt = BSS.Parameters.ff14SB(mdm2_wt, ensure_compatible=False).getMolecule()
mdm2_mut = BSS.Parameters.ff14SB(mdm2_mut, ensure_compatible=False).getMolecule()

mapping = BSS.Align.roiMatch(molecule0=mdm2_wt, molecule1=mdm2_mut, roi=[9])
aligned_wt = BSS.Align.roiAlign(molecule0=mdm2_wt, molecule1=mdm2_mut, roi=[9])
merged = BSS.Align.merge(molecule0=aligned_wt, molecule1=mdm2_mut, mapping=mapping, roi=[9])
```
A minimal code example with input files is also provided:  [minimal_pfep_example.tar.gz](https://github.com/OpenBioSim/biosimspace/files/15392209/minimal_pfep_example.tar.gz)

While in theory the `roiMatch` and `roiAlign` functions could be incorporated to already existing `matchAtoms` and `rmsdAlign` functions to have ROI functionality (in the way that `merge` function has ROI functionality and not a separate `roiMerge` function), I opted to have them as separate functions essentially to eliminate any risk of inadvertently modifying their behaviour and introducing bugs down the line. I think that this approach is safer overall, even if it introduces more functions to the Align module.

Another thing to note is that `viewMapping` function will be quite useless with this current ROI implementation since the mappings provided to the function will usually be quite large, in the future I would like to modify this function to allow mapping viewing between ROI residues. It's also important to note that I haven't tested this implementation with every possible canonical amino acid mutation, therefore it should be still treated as somewhat experimental, although I do believe the overall workflow will work correctly and any potential issues would arise from how the residues are mapped with the backend MCS function. Proline mutations are of course still not supported.
___

* I confirm that I have merged the latest version of `devel` into this branch before issuing this pull request (e.g. by running `git pull origin devel`): :white_check_mark:
* I confirm that I have added a test for any new functionality in this pull request: :x: - I still need to write automated tests for the new code, although the code does contain some sanity checks to test whether same residues are being mapped, etc.
* I confirm that I have added documentation (e.g. a new tutorial page or detailed guide) for any new functionality in this pull request: :x: - This still needs to be done too. 
* I confirm that I have permission to release this code under the GPL3 license: :white_check_mark:

## Suggested reviewers:
@lohedges,

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3df86f9ef01d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.