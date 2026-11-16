# Issue

**Title:** chore: add documentation dependencies to pixi and update build from source documentation.

## Description

## Description
Add the necessary package dependencies to pixi.toml and change the documentation sections for build from source, run tests, and dependencies to align with the changes. 

## Motivation and Context
As is, documentation is unable to be built with the pixi.toml file. Also, the build from source documentation had confusing elements. 

## How Has This Been Tested?
Used make html and received no warnings when building the new documentation. Ran pixi install with the new pixi.toml file and received no warnings and it built the documentation with the added packages. 
Followed the new documentation exactly to build and test the documentation and new pixi environment. 

## Checklist:
- [x] I have reviewed the [**Contributor Guidelines**](https://github.com/glotzerlab/freud/blob/main/CONTRIBUTING.md).
- [x] I agree with the terms of the [**Freud Contributor Agreement**](https://github.com/glotzerlab/freud/blob/main/ContributorAgreement.md).
- [x] My name is on the [list of contributors](https://github.com/glotzerlab/freud/blob/main/doc/source/reference/credits.rst) in the pull request source branch.
- [x] I have updated the [Change log](https://github.com/glotzerlab/freud/blob/main/ChangeLog.md).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e8ff280f1620`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.