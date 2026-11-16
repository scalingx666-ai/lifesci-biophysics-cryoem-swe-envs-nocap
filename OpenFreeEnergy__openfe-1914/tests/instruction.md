# Issue

**Title:** Only warn if forces mismatch in hash check

## Description

This unfortunately makes system checking much weaker. I don't think there's much of a way around though:
1. There's no way for us to know which two forces should match (i.e. we can't rely on names or force groups).
2. Small changes in floats will change the hash.

That being said, the exact System check is overly cautious. Generally, this mismatch would happen (where the particle numbers match and the number of forces match and the barostat matches) in cases where someone is actively trying to do something bad (i.e. the risk of it happening accidentally is low to non-existent).

Checklist
* [x] All new code is appropriately documented (user-facing code _must_ have complete docstrings).
* [x] Added a ``news`` entry, or the changes are not user-facing.
* [x] Ran pre-commit: you can run [pre-commit](https://pre-commit.com) locally or comment on this PR with `pre-commit.ci autofix`.

Manual Tests: these are slow so don't need to be run every commit, only before merging and when relevant changes are made (generally at reviewer-discretion). 
* [x] [GPU integration tests](https://github.com/OpenFreeEnergy/openfe/actions/workflows/aws-gpu-integration-tests.yaml)
* [ ] [example notebook testing](https://github.com/OpenFreeEnergy/openfe/actions/workflows/release-prep-examplenotebooks.yaml)
* [ ] [packaging tests](https://github.com/OpenFreeEnergy/openfe/actions/workflows/cron-package-test.yaml): run this for any large feature PRs or PRs that add test data.


## Developers certificate of origin
- [x] I certify that this contribution is covered by the MIT License [here](https://github.com/OpenFreeEnergy/openfe/blob/main/LICENSE) and the **Developer Certificate of Origin** at <https://developercertificate.org/>.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8c088e7ada3c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.