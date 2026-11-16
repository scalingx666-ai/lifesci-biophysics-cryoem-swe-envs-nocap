# Issue

**Title:** epic: add warehouse

## Description

<!--
Thank you for pull request.
Below are a few things we ask you kindly to self-check before getting a review. Remove checks that are not relevant.
-->

This introduces the concept of a `warehouse` (name to be workshopped?), which replaces the need for `metadatastore` , `resultclient` and `resultserver` functionality.


I do not consider this to be breaking compatibility, since we're removing unused code (that was not in our docs) and adding new functionality.


<!--
Please note any issues this fixes using [closing keywords]( https://help.github.com/articles/closing-issues-using-keywords/ ):
-->

<!--
see https://regro.github.io/rever-docs/news.html for details on how to add news entry (you do not need to run the rever command)
-->

Checklist
* [x] All new code is appropriately documented (user-facing code _must_ have complete docstrings): see  for separate docs work
* [x] Added a ``news`` entry, or the changes are not user-facing: this will come when merging into main
* [x] Ran pre-commit: you can run [pre-commit](https://pre-commit.com) locally or comment on this PR with `pre-commit.ci autofix`.

Manual Tests: these are slow so don't need to be run every commit, only before merging and when relevant changes are made (generally at reviewer-discretion). 
* [ ] [GPU integration tests](https://github.com/OpenFreeEnergy/openfe/actions/workflows/gpu-integration-tests.yaml)
* [ ] [example notebook testing](https://github.com/OpenFreeEnergy/openfe/actions/workflows/test-example-notebooks.yaml)
* [ ] [packaging tests](https://github.com/OpenFreeEnergy/openfe/actions/workflows/test-feedstock-pkg-build.yaml): run this for any large feature PRs or PRs that add test data.


## Developers certificate of origin
- [x] I certify that this contribution is covered by the MIT License [here](https://github.com/OpenFreeEnergy/openfe/blob/main/LICENSE) and the **Developer Certificate of Origin** at <https://developercertificate.org/>.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b3becadb96ff`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.