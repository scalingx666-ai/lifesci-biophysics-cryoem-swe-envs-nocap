# Issue

**Title:** Improve robustness of restraint search and fix torsional force constant bug

## Description

# Is this pull request to fix a bug, or to introduce new functionality?

This PR does two things:

1. Fixes 
2. Improves the robustness of the ABFE restraint search to strange user input. I've implemented the improvements discussed . 

I've included the two together to reduce CI runs/ faff, but please let me know if I should create separate PRs in future.

## If this is to fix a bug...

This pull request fixes issue #359?

* I confirm that I have merged the latest version of `devel` into this branch before issuing this pull request (e.g. by running `git pull origin devel`): [y]
* I confirm that I have permission to release this code under the GPL3 license: [y]

Note that the ordering of candidate anchor points in the restraint search will be slightly different now, as the torsions have less weight when ordering the restraints for selection.

## If this introduces new functionality...

Changes proposed in this pull request:

I now:

 - Ensure that anchors can never be in the same molecule
 - Warn the user if they select the protein/water as the "ligand"
 - Warn the user if they supply very few frames for fitting

* I confirm that I have merged the latest version of `devel` into this branch before issuing this pull request (e.g. by running `git pull origin devel`): [y]
* I confirm that I have added a test for any new functionality in this pull request: [y]
* I confirm that I have added documentation (e.g. a new tutorial page or detailed guide) for any new functionality in this pull request: [n]
* I confirm that I have permission to release this code under the GPL3 license: [y]

The tests could be more comprehensive - at the moment I've only added a test to check that an error is raised if the receptor and ligand candidate anchor point lists share atoms. I haven't included tests to check that the correct warnings are being raised. The reason for this was that the only trajectory I can find on the website (https://github.com/OpenBioSim/biosimspace_website/tree/main/structures) only includes the ligand and the protein. This means that I can't select weird "ligands" which aren't the ligand or protein (e.g. water) which don't raise errors for other reasons. If we'd like more complete tests, we could always add a trajectory with e.g. 1 water molecule as well as the complex. However, I have verified that these warnings are raised for one of my test systems, and the code for these checks is very simple.

## Suggested reviewers:
@lohedges 

Thanks.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `eb44a6fde5c2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.