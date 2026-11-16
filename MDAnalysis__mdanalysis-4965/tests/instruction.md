# Issue

**Title:** Update ResidueGroup/SegmentGroup with ids (PDBParser: force chainid to segid / Universe: set groups)

## Description

<!-- 
Insert issue number that this PR fixes (if any) just after 'Fixes #'.
If this PR does not fix an existing issue, consider opening one or remove 'Fixes #' from the PR description.
-->
 #2874

Changes made in this Pull Request:
<!-- Describe the changes that this PR makes. If applicable, use the following bullet list. --> 
- deal with conflicts between chainids and segids 
    - generate warnings when chainids and segids are different
    - generate warnings if chainids is prior to segids when loading into the `AtomGroup`/`ResidueGroup`/`SegmentGroup` in the universe
    - add an argument `force_chainids_to_segids` in `PDBReader` to forcibly reading the chain ID into the segment ID (PDB specific method)
    - add a function in the universe `set_groups` to update the topology and groups in the universe when atomwise resids/segids is given (more generalized method across different file type)

## PR Checklist
<!-- Please use the following checklist to ensure the PR is ready to be reviewed/merged. -->
 - [x] Issue raised/referenced?
 - [x] Tests updated/added?
 - [x] Documentation updated/added?
 - [x] `package/CHANGELOG` file updated?
 - [x] Is your name in `package/AUTHORS`? (If it is not, add it!)

## Developers Certificate of Origin
<!-- 
In order for this PR to be merged you **must certify that you are able to submit your code to be included in MDAnalysis**.
-->

I certify that I can submit this code contribution as described in the [**Developer Certificate of Origin**](https://developercertificate.org/), under the MDAnalysis [LICENSE](https://github.com/MDAnalysis/mdanalysis/blob/develop/LICENSE).


<!-- readthedocs-preview mdanalysis start -->
----
📚 Documentation preview 📚: https://mdanalysis--4965.org.readthedocs.build/en/4965/

<!-- readthedocs-preview mdanalysis end -->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `740332c8fb4f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.