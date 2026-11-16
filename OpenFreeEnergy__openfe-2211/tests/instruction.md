# Issue

**Title:** task-based execution user guide edits

## Description

<!--
Thank you for pull request.
Below are a few things we ask you kindly to self-check before getting a review. Remove checks that are not relevant.
-->
## Description
<!--
Please note any issues this fixes using [closing keywords]( https://help.github.com/articles/closing-issues-using-keywords/ ):
-->

from @hannahbaumann addressed in this PR:

- [x] Since the vocabulary of tasks and task-based execution will be new to a lot of our users, it would be great to introduce them in the user guide.
- [x] This part in the user guide is confusing, since the default alchemical network output file name is alchemicalNetwork.json, but here we're switching to tyk2.json, even though the paragraph above says the output is alchemicalNetwork.json.
  >  Once you have an AlchemicalNetwork, use the following command to set up the task-based campaign. By default, the TaskDB and Warehouse will be created using the input file basename (here, tyk2).
- [x] Re parallel submission of multiple jobs: I think here we could also mention the use of array jobs, e.g. sbatch --array=1-100 run_tasks.sh which would make parallel execution more practical then manually submitting the same job many times.
- [x] Maybe the user guide could also briefly explain where run-task stores the results? (including the shared and scratch directories?)



<!--
see https://regro.github.io/rever-docs/news.html for details on how to add news entry (you do not need to run the rever command)
-->

## LLM / AI generated code disclosure
<!-- Please update this disclosure to reflect if you did or did not use LLMs / AI to generate code -->
LLMs or other AI-powered tools (beyond simple IDE use cases) were used in this contribution: no

## Checklist
* [x] All new code is appropriately documented (user-facing code _must_ have complete docstrings).
* [x] Added a ``news`` entry, or the changes are not user-facing.
* [x] Ran pre-commit: you can run [pre-commit](https://pre-commit.com) locally or comment on this PR with `pre-commit.ci autofix`.
* [x] Adhered to the [contribution guidelines](https://github.com/OpenFreeEnergy/openfe?tab=contributing-ov-file#contribution-guidelines)
* [x] Filled in the AI generated code disclosure.

Manual Tests: these are slow so don't need to be run every commit, only before merging and when relevant changes are made (generally at reviewer-discretion). 
* [ ] [GPU integration tests](https://github.com/OpenFreeEnergy/openfe/actions/workflows/aws-gpu-integration-tests.yaml)
* [ ] [example notebook testing](https://github.com/OpenFreeEnergy/openfe/actions/workflows/release-prep-examplenotebooks.yaml)
* [ ] [packaging tests](https://github.com/OpenFreeEnergy/openfe/actions/workflows/cron-package-test.yaml): run this for any large feature PRs or PRs that add test data.

## Developers certificate of origin
- [x] I certify that this contribution is covered by the MIT License [here](https://github.com/OpenFreeEnergy/openfe/blob/main/LICENSE) and the **Developer Certificate of Origin** at <https://developercertificate.org/>.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8c06184f7669`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.