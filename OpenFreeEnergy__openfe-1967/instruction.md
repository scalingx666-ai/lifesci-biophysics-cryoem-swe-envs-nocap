# Issue

**Title:** release prep for v1.11.0

## Description

<!--
Checklist for releasing a new version of openfe. 
-->
NOTE: we may want to merge in  for v1.11.0, but I'm starting this branch to begin pre-release testing.

Make the PR:
* [x] Create a new release prep branch corresponding to the version name, e.g. `release/v1.2.0`.  Note: please follow [semantic versioning](https://semver.org/).
* [x] Check that all user-relevant updates are included in the `news/` rever `.rst` files. You can backfill any additional items by making a new .rst, e.g. `backfill.rst`
* [x] Run [rever](https://regro.github.io/rever-docs/index.html#), e.g. `rever 1.2.0`. This will auto-commit `docs/CHANGELOG.md` and remove the `.rst` files from `news/`. 
* [x] Verify that`docs/CHANGELOG.rst` looks correct and that it renders as expected in the docs preview.
* [x] If needed, create a release of the [example notebooks repository](https://github.com/OpenFreeEnergy/ExampleNotebooks) and update the pinned release version in the `openfe/docs/conf.py`.
* [x] Make the PR and verify that CI/CD passes. 
  * [x] [feedstock packaging tests](https://github.com/OpenFreeEnergy/openfe/actions/workflows/release-prep-feedstock.yaml)
  * [x] [example notebooks](https://github.com/OpenFreeEnergy/openfe/actions/workflows/release-prep-examplenotebooks.yaml)
  * [x] [GPU tests](https://github.com/OpenFreeEnergy/openfe/actions/workflows/aws-gpu-integration-tests.yaml)
* [x] Merge the PR into `main`.

After Merging the PR [follow this guide](https://github.com/OpenFreeEnergy/openfe/wiki/How-to-create-a-new-release)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7bc31e1df5b3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.