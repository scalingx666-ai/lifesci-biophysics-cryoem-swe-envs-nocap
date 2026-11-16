# Issue

**Title:** Speed up HTF creation

## Description

<!--
Thank you for pull request.
Below are a few things we ask you kindly to self-check before getting a review. Remove checks that are not relevant.
-->
Try to speed up the HTF creation, targeting the torsion force and nonbonded exceptions as they scale terribly with system size. Local testing on a small protein-ligand system shows good improvements

Main
> systems built starting test
Handled harmonic bonds in 0.36160308588296175 seconds
Handled harmonic angles in 1.0089957918971777 seconds
Handled periodic torsions in 7.016903054900467 seconds
Handled nonbonded interactions in 25.400564597919583 seconds
HybridTopologyFactory took 37.79149968735874 seconds

This branch
> systems built starting test
Handled harmonic bonds in 0.3190057808533311 seconds
Handled harmonic angles in 0.9470380647107959 seconds
Handled periodic torsions in 0.24904045276343822 seconds
Time to handle original exceptions 0.51 seconds
Handled nonbonded interactions in 2.244331883266568 seconds
Handled nonbonded exceptions in 0.04826593864709139 seconds
HybridTopologyFactory took 7.013191535137594 seconds


We probably need the tests in #1856 to confirm that nothing is broken with these changes.


## TODO
- test on membrane system 
<!--
Please note any issues this fixes using [closing keywords]( https://help.github.com/articles/closing-issues-using-keywords/ ):
-->

<!--
see https://regro.github.io/rever-docs/news.html for details on how to add news entry (you do not need to run the rever command)
-->

Checklist
* [ ] All new code is appropriately documented (user-facing code _must_ have complete docstrings).
* [ ] Added a ``news`` entry, or the changes are not user-facing.
* [x] Ran pre-commit: you can run [pre-commit](https://pre-commit.com) locally or comment on this PR with `pre-commit.ci autofix`.

Manual Tests: these are slow so don't need to be run every commit, only before merging and when relevant changes are made (generally at reviewer-discretion). 
* [ ] [GPU integration tests](https://github.com/OpenFreeEnergy/openfe/actions/workflows/gpu-integration-tests.yaml)
* [ ] [example notebook testing](https://github.com/OpenFreeEnergy/openfe/actions/workflows/test-example-notebooks.yaml)
* [ ] [packaging tests](https://github.com/OpenFreeEnergy/openfe/actions/workflows/test-feedstock-pkg-build.yaml): run this for any large feature PRs or PRs that add test data.


## Developers certificate of origin
- [x] I certify that this contribution is covered by the MIT License [here](https://github.com/OpenFreeEnergy/openfe/blob/main/LICENSE) and the **Developer Certificate of Origin** at <https://developercertificate.org/>.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9b2a705e40b8`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.