# Issue

**Title:** Feature custom roi maps

## Description

This PR adds `custom_roi_map` functionality to `matchAtoms()` function as well as the underlying `_roiMatch()` function in order to enable user defined mappings for ROI regions. This allows for creation of protein perturbations involving advanced use-cases, such as ring-breaks for example.  The argument only works when `roi` is passed to `matchAtoms()`, so the underlying behaviour of the function outside of protein FEP remains unchanged.

For example as detailed in the tutorial,  `custom_roi_map={}`  can be used to return an empty mapping for ROI region:
```python
mapping = BSS.Align.matchAtoms(molecule0=wt, molecule1=mut, roi=[15], custom_roi_map={})
BSS.Align.viewMapping(wt, mut, mapping, roi=15, pixels=500)
```
<img width="1000" height="500" alt="image" src="https://github.com/user-attachments/assets/022086ab-01c9-4fdc-88fb-5d12c20fdc0e" />

Or pass a custom mapping where the RDKit MCS algorithm would fail otherwise:
```python
mapping = BSS.Align.matchAtoms(molecule0=wt, molecule1=mut, roi=[15], custom_roi_map={204:204,205:205,203:203,202:202,211:208,206:206,213:210,207:207,214:211,210:213})

BSS.Align.viewMapping(wt, mut, mapping, roi=15, pixels=500)
```
<img width="1000" height="500" alt="image" src="https://github.com/user-attachments/assets/ca547c32-506f-4b73-b44e-41b1896d9af2" />

___

Specifically this PR:
1. Adds the `custom_roi_map` functionality as described above.
2. Introduces `test_empty_custom_roi_mapping` and `test_custom_roi_ring_break_merge` test cases in [test_align.py](https://github.com/akalpokas/biosimspace/blob/feature_custom_ROI_maps/tests/Align/test_align.py) that test the following:
- `test_empty_custom_roi_mapping`: that a user can pass an empty `custom_roi_map` dictionary to return two unmapped ROI residues, while the rest of the protein is mapped correctly. This allows for setting up transformations using a "dual-topology" approach.
-  `test_custom_roi_ring_break_merge`: that a user can pass a custom mapping to create an alchemical ring transformation in a test system. The tests checks for this by asserting the number of alchemically created bonds as part of the mapping is equal to 1 (Leucine to Proline transformation is reverse of a ring-break).
3. Updates the [protein FEP tutorial](https://github.com/akalpokas/biosimspace/blob/feature_custom_ROI_maps/doc/source/tutorials/protein_mutations.rst) to demonstrate the new functionality

* I confirm that I have merged the latest version of `devel` into this branch before issuing this pull request (e.g. by running `git pull origin devel`): y
* I confirm that I have added a test for any new functionality in this pull request: y
* I confirm that I have added documentation (e.g. a new tutorial page or detailed guide) for any new functionality in this pull request: y
* I confirm that I have permission to release this code under the GPL3 license: y

## Suggested reviewers:
@lohedges

## Any additional context of information?
This PR is non release blocking and can be merged after the release if needed.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `07765cb28abf`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.