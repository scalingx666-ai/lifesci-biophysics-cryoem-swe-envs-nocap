# Issue

**Title:** Fix PMFT plot and add cmap option

## Description

<!-- Provide a general summary of your changes in the Title above. -->

## Description, Motivation, and Context

PMFTXY.plot() called for the `self.PMFT`, `self.X`, and `self.Y` properties which were renamed to `self.pmft` and `self.bounds` in a recent PR. This changes fixes these calls, and adds an optional "cmap" parameter that passes through into the internal call to `matplotlib.imshow`.

## How Has This Been Tested?

Used with cmap="afmhot".

<img width="624" alt="image" src="https://github.com/user-attachments/assets/f801762f-1506-4228-b5a7-a49413162b81" />


## Checklist:
- [x] I have reviewed the [**Contributor Guidelines**](https://github.com/glotzerlab/freud/blob/main/CONTRIBUTING.md).
- [x] I agree with the terms of the [**Freud Contributor Agreement**](https://github.com/glotzerlab/freud/blob/main/ContributorAgreement.md).
- [x] My name is on the [list of contributors](https://github.com/glotzerlab/freud/blob/main/doc/source/reference/credits.rst) in the pull request source branch.
- [ ] I have updated the [Change log](https://github.com/glotzerlab/freud/blob/main/ChangeLog.md).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `007f5e343036`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.