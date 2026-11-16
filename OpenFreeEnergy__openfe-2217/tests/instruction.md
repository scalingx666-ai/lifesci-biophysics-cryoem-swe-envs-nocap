# Issue

**Title:** Return self in model_validator for BoreschRestraintSettings

## Description

My understanding of the `model_validator` docs is that we should return self at the end. It looks like this is the cause of the issue (not tried it locally yet).

Relevant docs section:
```
After validators: run after the whole model has been validated. As such, they are defined as instance methods and can be seen as post-initialization hooks. Important note: the validated instance should be returned.After validators: run after the whole model has been validated. As such, they are defined as instance methods and can be seen as post-initialization hooks. Important note: the validated instance should be returned.
```

## LLM / AI generated code disclosure
<!-- Please update this disclosure to reflect if you did or did not use LLMs / AI to generate code -->
LLMs or other AI-powered tools (beyond simple IDE use cases) were used in this contribution: no
If yes, please provide details here:

## Checklist
* [ ] All new code is appropriately documented (user-facing code _must_ have complete docstrings).
* [ ] Added a ``news`` entry, or the changes are not user-facing.
* [ ] Ran pre-commit: you can run [pre-commit](https://pre-commit.com) locally or comment on this PR with `pre-commit.ci autofix`.
* [ ] Adhered to the [contribution guidelines](https://github.com/OpenFreeEnergy/openfe?tab=contributing-ov-file#contribution-guidelines)
* [ ] Filled in the AI generated code disclosure.

Manual Tests: these are slow so don't need to be run every commit, only before merging and when relevant changes are made (generally at reviewer-discretion). 
* [ ] [GPU integration tests](https://github.com/OpenFreeEnergy/openfe/actions/workflows/aws-gpu-integration-tests.yaml)
* [ ] [example notebook testing](https://github.com/OpenFreeEnergy/openfe/actions/workflows/release-prep-examplenotebooks.yaml)
* [ ] [packaging tests](https://github.com/OpenFreeEnergy/openfe/actions/workflows/cron-package-test.yaml): run this for any large feature PRs or PRs that add test data.

## Developers certificate of origin
- [x] I certify that this contribution is covered by the MIT License [here](https://github.com/OpenFreeEnergy/openfe/blob/main/LICENSE) and the **Developer Certificate of Origin** at <https://developercertificate.org/>.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `18208178d084`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.