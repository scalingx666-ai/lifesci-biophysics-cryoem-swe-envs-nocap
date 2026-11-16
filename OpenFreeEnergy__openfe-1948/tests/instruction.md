# Issue

**Title:** Switch repeat id assignment from leg to repeat in AFE Protocols

## Description

This PR changes things so that a single uuid is used for each repeat in a Protocol, rather than each leg. This more strictly adheres to the idea of being a "repeat_id". It is also more intuitive since it allows us to assume that the uuid represents a single DAG execution.

We may have to revisit this formalism if/when we have a case where we have multiple indentical units being executed in a single DAG.

Checklist
* [x] All new code is appropriately documented (user-facing code _must_ have complete docstrings).
* [x] Added a ``news`` entry, or the changes are not user-facing.
* [x] Ran pre-commit: you can run [pre-commit](https://pre-commit.com) locally or comment on this PR with `pre-commit.ci autofix`.

Manual Tests: these are slow so don't need to be run every commit, only before merging and when relevant changes are made (generally at reviewer-discretion). 
* [x] [GPU integration tests](https://github.com/OpenFreeEnergy/openfe/actions/workflows/aws-gpu-integration-tests.yaml) - ran locally
* [ ] [example notebook testing](https://github.com/OpenFreeEnergy/openfe/actions/workflows/release-prep-examplenotebooks.yaml)
* [ ] [packaging tests](https://github.com/OpenFreeEnergy/openfe/actions/workflows/cron-package-test.yaml): run this for any large feature PRs or PRs that add test data.


## Developers certificate of origin
- [x] I certify that this contribution is covered by the MIT License [here](https://github.com/OpenFreeEnergy/openfe/blob/main/LICENSE) and the **Developer Certificate of Origin** at <https://developercertificate.org/>.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `cb10892e79a2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.