# Issue

**Title:** ENH: Change DSSP `get_hbond_map` to use capped_distance and PBC-aware distance calculations

## Description

Changes made in this Pull Request:

This updates the `get_hbond_map` function to use `MDAnalysis.lib.distances.capped_distance`. This can now respect periodic boundary crossing for proper calculation of SS across boundaries. The `backend` option is now also supported for `distopia` and `openmp`. Nothing changes in the API except for the additional `backend` argument.

Rather than computing a pairwise distance matrix of every residue against every other residue, we can utilised KDTrees first with `capped_distance` to find potential pairs that also looks across boundaries and use `calc_bonds` for accelerated distance calculations that also respect boundaries.

The returned value from `get_hbond_map` remains the same but we have avoided having to compute distances and energies between residues that would never form an HBond to begin with.

## PR Checklist
<!-- Please use the following checklist to ensure the PR is ready to be reviewed/merged. -->
 - [ ] Issue raised/referenced?
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
📚 Documentation preview 📚: https://mdanalysis--5182.org.readthedocs.build/en/5182/

<!-- readthedocs-preview mdanalysis end -->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9273f9b9dbf7`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.