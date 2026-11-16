# Issue

**Title:** Fix bugs in smart defaults for sequences and numpy arrays

## Description

## Description

Small changes to resolve bugged behavior with smart defaults when dealing with sequences and numpy arrays. The changes are as follows:

1. In [`hoomd.data.smart_default._SmartDefaultSequence`](https://github.com/glotzerlab/hoomd-blue/blob/ccf032b2deb1c2f7e5a40c33861d5b546fe40ec0/hoomd/data/smart_default.py#L32-L73) (the class for getting, setting, and applying default values for typeparams that expect sequences), `__call__` has been redefined to simply return the provided sequence if it is not None and its length is greater than 1. Previously, a new sequence was built up item-by-item by iterating over the default sequence, which constrained the returned sequence to the length of the default.

2. In [`hoomd.data.smart_default._to_default`](https://github.com/glotzerlab/hoomd-blue/blob/ccf032b2deb1c2f7e5a40c33861d5b546fe40ec0/hoomd/data/smart_default.py#L159-L178) (the function for constructing smart default instances for a value based on its type), a new control flow branch has been added to simply return the provided value if it is a numpy array. This was actually already expected behavior (see [this comment](https://github.com/glotzerlab/hoomd-blue/blob/ccf032b2deb1c2f7e5a40c33861d5b546fe40ec0/hoomd/data/collections.py#L592-L595)), but no tests were ever written to enforce it.
 
## Motivation and context

This PR  and #2289, which both involve unexpected behavior around setting default values for typeparams that expect sequence values.

## How has this been tested?

Spot tests have been added to [`hoomd/md/pytest/test_alj.py`](https://github.com/glotzerlab/hoomd-blue/blob/5b2782e414c158da68d99746465b7ccded7b2550/hoomd/md/pytest/test_alj.py) and the new file [`hoomd/md/pytest/test_table_default_array.py`](https://github.com/glotzerlab/hoomd-blue/blob/5b2782e414c158da68d99746465b7ccded7b2550/hoomd/md/pytest/test_table_default_array.py). These tests use the MWEs from #2190 and #2289.

These changes do not happen in the public API, so docs testing should not be necessary.

## Checklist:

- [x] I have reviewed the [**Contributor Guidelines**](https://github.com/glotzerlab/hoomd-blue/blob/trunk/CONTRIBUTING.rst).
- [x] I agree with the terms of the [**HOOMD-blue Contributor Agreement**](https://github.com/glotzerlab/hoomd-blue/blob/trunk/ContributorAgreement.md).
- [x] My name is on the list of contributors (`sphinx-doc/credits.rst`) in the pull request source branch.
- [x] I have summarized these changes in `CHANGELOG.rst` following the established format.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ccf032b2deb1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.