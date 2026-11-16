# Issue

**Title:** Require at least 4 mapped heavy atoms in the Hybrid Topology protocol

## Description

<!--
Thank you for pull request.
Below are a few things we ask you kindly to self-check before getting a review. Remove checks that are not relevant.
-->
, we now require at least 3 heavy atoms to be mapped in the protocol to lead to more stable simulations and log information about the mapping during validation. 

<!--
Please note any issues this fixes using [closing keywords]( https://help.github.com/articles/closing-issues-using-keywords/ ):
-->

<!--
see https://regro.github.io/rever-docs/news.html for details on how to add news entry (you do not need to run the rever command)
-->

## LLM / AI generated code disclosure
<!-- Please update this disclosure to reflect if you did or did not use LLMs / AI to generate code -->
LLMs or other AI-powered tools (beyond simple IDE use cases) were used in this contribution: yes / no
If yes, please provide details here: No

Checklist
* [ ] All new code is appropriately documented (user-facing code _must_ have complete docstrings).
* [x] Added a ``news`` entry, or the changes are not user-facing.
* [x] Ran pre-commit: you can run [pre-commit](https://pre-commit.com) locally or comment on this PR with `pre-commit.ci autofix`.
* [x] Filled in the AI generated code disclosure.

Manual Tests: these are slow so don't need to be run every commit, only before merging and when relevant changes are made (generally at reviewer-discretion). 
* [ ] [GPU integration tests](https://github.com/OpenFreeEnergy/openfe/actions/workflows/aws-gpu-integration-tests.yaml)
* [ ] [example notebook testing](https://github.com/OpenFreeEnergy/openfe/actions/workflows/release-prep-examplenotebooks.yaml)
* [ ] [packaging tests](https://github.com/OpenFreeEnergy/openfe/actions/workflows/cron-package-test.yaml): run this for any large feature PRs or PRs that add test data.

## Developers certificate of origin
- [x] I certify that this contribution is covered by the MIT License [here](https://github.com/OpenFreeEnergy/openfe/blob/main/LICENSE) and the **Developer Certificate of Origin** at <https://developercertificate.org/>.

## Question
- Do we want to make this minimum bound configurable somehow?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d8a6d3712b7c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.