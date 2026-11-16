# Issue

**Title:** Refactor residue-name matching logic in selection.py

## Description

<!-- 
Insert issue number that this PR fixes (if any) just after 'Fixes #'.
If this PR does not fix an existing issue, consider opening one or remove 'Fixes #' from the PR description.
-->



Changes made in this Pull Request:
<!-- Describe the changes that this PR makes. If applicable, use the following bullet list. --> 
- Introduced a helper function Selection._apply_match_by_resnames() to centralize residue-name matching logic.
- Refactored various classes to use the new helper method instead of implementing similar logic independently.
- Removed duplicated code related to residue-name matching from these selection classes.
- Verified that existing tests pass to ensure no change in behavior.

This refactor improves code maintainability by reducing duplication and providing a reusable utility for selections that operate on residue names.

## LLM / AI generated code disclosure
LLMs or other AI-powered tools (beyond simple IDE use cases) were used in this contribution: no

## PR Checklist
<!-- Please use the following checklist to ensure the PR is ready to be reviewed/merged. -->
 - [x] Issue raised/referenced?
 - [x] Tests updated/added? (NA)
 - [x] Documentation updated/added?
 - [x] package/CHANGELOG file updated? 
 - [x] Is your name in package/AUTHORS?
 - [x] LLM/AI disclosure was updated.

## Developers Certificate of Origin
<!-- 
In order for this PR to be merged you **must certify that you are able to submit your code to be included in MDAnalysis**.
-->

I certify that I can submit this code contribution as described in the [**Developer Certificate of Origin**](https://developercertificate.org/), under the MDAnalysis [LICENSE](https://github.com/MDAnalysis/mdanalysis/blob/develop/LICENSE).


<!-- readthedocs-preview mdanalysis start -->
----
📚 Documentation preview 📚: https://mdanalysis--5294.org.readthedocs.build/en/5294/

<!-- readthedocs-preview mdanalysis end -->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6e837a4eb027`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.