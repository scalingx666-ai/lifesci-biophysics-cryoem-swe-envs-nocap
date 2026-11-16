# Issue

**Title:** Use unbiased std for uncertainty estimates

## Description

<!--
Thank you for pull request.
Below are a few things we ask you kindly to self-check before getting a review. Remove checks that are not relevant.
-->
Updates the uncertainty functions on protocol results to return the unbiased estimate of the standard deviation from the sample population.

# Questions
- Would it be better to return the `SE = std / sqrt(n)` which is the uncertainty in the estimate of the average rather than the spread of the data? Where std is the unbiased estimate? 
- What do we do when `n=1` the uncertainty will now return Nan? 

<!--
Please note any issues this fixes using [closing keywords]( https://help.github.com/articles/closing-issues-using-keywords/ ):
-->

<!--
see https://regro.github.io/rever-docs/news.html for details on how to add news entry (you do not need to run the rever command)
-->

Checklist
* [x] All new code is appropriately documented (user-facing code _must_ have complete docstrings).
* [x] Added a ``news`` entry, or the changes are not user-facing.
* [ ] Ran pre-commit: you can run [pre-commit](https://pre-commit.com) locally or comment on this PR with `pre-commit.ci autofix`.

Manual Tests: these are slow so don't need to be run every commit, only before merging and when relevant changes are made (generally at reviewer-discretion). 
* [ ] [GPU integration tests](https://github.com/OpenFreeEnergy/openfe/actions/workflows/aws-gpu-integration-tests.yaml)
* [ ] [example notebook testing](https://github.com/OpenFreeEnergy/openfe/actions/workflows/release-prep-examplenotebooks.yaml)
* [ ] [packaging tests](https://github.com/OpenFreeEnergy/openfe/actions/workflows/cron-package-test.yaml): run this for any large feature PRs or PRs that add test data.


## Developers certificate of origin
- [ ] I certify that this contribution is covered by the MIT License [here](https://github.com/OpenFreeEnergy/openfe/blob/main/LICENSE) and the **Developer Certificate of Origin** at <https://developercertificate.org/>.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `58d1e008610c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.