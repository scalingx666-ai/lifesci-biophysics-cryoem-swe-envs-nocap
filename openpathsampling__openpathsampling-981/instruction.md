# Issue

**Title:** Remove some Py2-specific code

## Description

#977 dropped Python 2 from the OPS 2.0 test matrix. In the process, some code lost coverage. Here I remove the Py2-specific code for most of that (except netcdfplus, which I'm not worrying about since it will also be dropped in 2.0).

There's one line where I don't immediately understand how it lost coverage:

https://github.com/openpathsampling/openpathsampling/blob/6ff2cbbf615928c031896e1d719217d2087fa940/openpathsampling/engines/dynamics_engine.py#L256

This is in an instance test against `basestring`, but `basestring` is `str` for Py3+ (we set it at the top of the file). Any idea why that's not getting covered? (particularly @sroet)

Note that this PR is not intended to drop all Py2-related code -- there's more to do on that. This is just catching the things that lost coverage when Py2 left the test matrix.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6ff2cbbf6159`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.