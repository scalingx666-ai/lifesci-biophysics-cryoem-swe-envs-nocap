# Issue

**Title:** Updating Examples in Documentation

## Description

## Description

Updated the example in integrate.py to comply with the current hoomd syntax and converted examples in hoomd/md so that they are tested with Sybil

## Motivation and context

The old example in integrate.py was from an older version of hoomd and the syntax was not functional in the current version of hoomd

Partially  
I am not sure if this catches all of the old examples

## How has this been tested?

The example in integrate.py was tested by following the tutorials in a jupyter notebook.
The sphinx documentation was rebuilt.

## Checklist:

- [ x] I have reviewed the [**Contributor Guidelines**](https://github.com/glotzerlab/hoomd-blue/blob/trunk/CONTRIBUTING.rst).
- [x ] I agree with the terms of the [**HOOMD-blue Contributor Agreement**](https://github.com/glotzerlab/hoomd-blue/blob/trunk/ContributorAgreement.md).

- [x] My name is on the list of contributors (`sphinx-doc/credits.rst`) in the pull request source branch.

- [ ] I have summarized these changes in `CHANGELOG.rst` following the established format.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2376ac52ca39`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.