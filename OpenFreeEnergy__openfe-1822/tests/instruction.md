# Issue

**Title:** fix ligand network cropping bug

## Description

<!--
Thank you for pull request.
Below are a few things we ask you kindly to self-check before getting a review. Remove checks that are not relevant.
-->
Thanks to the user who suggested this fix in #1800!

before:

<img width="810" height="867" alt="Screenshot 2026-02-02 at 2 24 46 PM" src="https://github.com/user-attachments/assets/4c3c7870-348b-4b46-b70e-072a550662b5" />


after:
<img width="790" height="859" alt="Screenshot 2026-02-02 at 2 31 41 PM" src="https://github.com/user-attachments/assets/ee3af28e-d9dd-4e63-a4f1-d4ad0ff2fddc" />



<!--
Please note any issues this fixes using [closing keywords]( https://help.github.com/articles/closing-issues-using-keywords/ ):
-->

<!--
see https://regro.github.io/rever-docs/news.html for details on how to add news entry (you do not need to run the rever command)
-->

Checklist
* [x] All new code is appropriately documented (user-facing code _must_ have complete docstrings).
* [x] Added a ``news`` entry, or the changes are not user-facing.
* [x] Ran pre-commit: you can run [pre-commit](https://pre-commit.com) locally or comment on this PR with `pre-commit.ci autofix`.

Manual Tests: these are slow so don't need to be run every commit, only before merging and when relevant changes are made (generally at reviewer-discretion). 
* [ ] [GPU integration tests](https://github.com/OpenFreeEnergy/openfe/actions/workflows/gpu-integration-tests.yaml)
* [ ] [example notebook testing](https://github.com/OpenFreeEnergy/openfe/actions/workflows/test-example-notebooks.yaml)
* [ ] [packaging tests](https://github.com/OpenFreeEnergy/openfe/actions/workflows/test-feedstock-pkg-build.yaml): run this for any large feature PRs or PRs that add test data.


## Developers certificate of origin
- [ ] I certify that this contribution is covered by the MIT License [here](https://github.com/OpenFreeEnergy/openfe/blob/main/LICENSE) and the **Developer Certificate of Origin** at <https://developercertificate.org/>.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `994b84e664d1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.