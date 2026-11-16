# Issue

**Title:** Add `to_box_params` method for exporting boxes to GSD

## Description

<!-- Provide a general summary of your changes in the Title above. -->

## Description

Freud boxes are typically initialized from `Lx, Ly, Lz, xy, xz, yz` data, but there is no neat way to recover that data from the input. This method (which ~could~ should? be renamed for clarity) provides that functionality.

## Motivation and Context

Initializing GSD boxes can now be achieved in one function call:
```diff
frame = gsd.hoomd.Frame()
- frame.configuration.box = [freud_box.Lx, freud_box.Ly, ..., freud_box.yz]
+ frame.configuration.box = freud_box.to_box_params()
```


## How Has This Been Tested?
New tests have been added

## Checklist:
- [x] I have reviewed the [**Contributor Guidelines**](https://github.com/glotzerlab/freud/blob/main/CONTRIBUTING.md).
- [x] I agree with the terms of the [**Freud Contributor Agreement**](https://github.com/glotzerlab/freud/blob/main/ContributorAgreement.md).
- [x] My name is on the [list of contributors](https://github.com/glotzerlab/freud/blob/main/doc/source/reference/credits.rst) in the pull request source branch.
- [x] I have updated the [Change log](https://github.com/glotzerlab/freud/blob/main/ChangeLog.md).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `007f5e343036`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.