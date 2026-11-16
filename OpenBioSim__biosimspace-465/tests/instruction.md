# Issue

**Title:** Add support for setting coordinates of all atoms from a NumPy array

## Description

This PR wraps the new Sire function to set coordinates of all atoms in a system from an array of coordinates. I've also added some tests that mirror those in the Sire test suite.

The code is intended to be quick, so has no validation other than the number of entries in the coordinates array matching the number of atoms in the system. Validation, e.g. comparing elements for each atom, could be done _before_ calling the function.

I had thought about adding `getVelocities` and `setVelocities` methods but unfortunately this can't easily be added by hacking the `sr.io.get_coords_array` function using the property map, i.e. mapping `coordinates` to `velocities`, since the two properties use different types under the hood. This could be added if desired, but would be a bit more work. My hope was that I could just add extra methods that called the coordinates ones using the map.

While this PR is raised as a "feature", so wouldn't normally be available until the next minor release, it is completely self-contained and doesn't affect any existing functionality. As such, it could be backported if there was a pressing need.

* I confirm that I have merged the latest version of `devel` into this branch before issuing this pull request (e.g. by running `git pull origin devel`): [y]
* I confirm that I have added a test for any new functionality in this pull request: [y]
* I confirm that I have added documentation (e.g. a new tutorial page or detailed guide) for any new functionality in this pull request: [y]
* I confirm that I have permission to release this code under the GPL3 license: [y]

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2cc1724f5915`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.