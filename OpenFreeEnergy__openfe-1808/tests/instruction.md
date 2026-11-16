# Issue

**Title:** Add resuming support in AFE Protocols

## Description

Similar to #1774 

Checklist
* [x] All new code is appropriately documented (user-facing code _must_ have complete docstrings).
* [ ] Added a ``news`` entry, or the changes are not user-facing.
* [x] Ran pre-commit: you can run [pre-commit](https://pre-commit.com) locally or comment on this PR with `pre-commit.ci autofix`.

Manual Tests: these are slow so don't need to be run every commit, only before merging and when relevant changes are made (generally at reviewer-discretion). 
* [x] [GPU integration tests](https://github.com/OpenFreeEnergy/openfe/actions/workflows/gpu-integration-tests.yaml)
* [ ] [example notebook testing](https://github.com/OpenFreeEnergy/openfe/actions/workflows/test-example-notebooks.yaml)
* [ ] [packaging tests](https://github.com/OpenFreeEnergy/openfe/actions/workflows/test-feedstock-pkg-build.yaml): run this for any large feature PRs or PRs that add test data.


## Developers certificate of origin
- [x] I certify that this contribution is covered by the MIT License [here](https://github.com/OpenFreeEnergy/openfe/blob/main/LICENSE) and the **Developer Certificate of Origin** at <https://developercertificate.org/>.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d69baa614bfc`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.