# Issue

**Title:** Implementation of fetch_pdb()

## Description

<!-- 
Insert issue number that this PR fixes (if any) just after 'Fixes #'.
If this PR does not fix an existing issue, consider opening one or remove 'Fixes #' from the PR description.
-->
 


Changes made in this Pull Request:
<!-- Describe the changes that this PR makes. If applicable, use the following bullet list. --> 
This is a still work in progress, but here's a implementation of @BradyAJohnston 's code wrapped into classes. I still need to write tests and docs for the entire thing.

- Added two classes: `DownloaderBase` and 'PDBDownloader' in order to implement downloading structure file from online sources such as the PDB databank.
-  Added `requests` as a dependency
- `mda.fetch_pdb()` is implemented as a wrapper to commonly used option in 'PDBDownloader'

## PR Checklist
<!-- Please use the following checklist to ensure the PR is ready to be reviewed/merged. -->
 - [x] Issue raised/referenced?
 - [x] Tests updated/added?
 - [x] Documentation updated/added?
 - [x] `package/CHANGELOG` file updated?


## Developers Certificate of Origin
<!-- 
In order for this PR to be merged you **must certify that you are able to submit your code to be included in MDAnalysis**.
-->

I certify that I can submit this code contribution as described in the [**Developer Certificate of Origin**](https://developercertificate.org/), under the MDAnalysis [LICENSE](https://github.com/MDAnalysis/mdanalysis/blob/develop/LICENSE).


<!-- readthedocs-preview mdanalysis start -->
----
📚 Documentation preview 📚: https://mdanalysis--4943.org.readthedocs.build/en/4943/

<!-- readthedocs-preview mdanalysis end -->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2f240fb12e5e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.