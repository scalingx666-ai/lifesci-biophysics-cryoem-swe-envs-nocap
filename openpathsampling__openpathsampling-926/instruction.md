# Issue

**Title:** Pin ujson<2; nbconvert<6

## Description

*  Pinning ujson < 2 . (@hejung -- this is correct, right?) Long term solution is that the new storage I'm working on doesn't require ujson.
*  Pinning nbconvert < 6 fixes a [problem in current builds of docs](https://travis-ci.org/github/openpathsampling/openpathsampling/jobs/727635606#L4066-L4075). Something fails and then hangs in our custom notebook conversion sphinx plugin (written by @jhprinz and no longer maintained). Long term solution is to move to nbsphinx.
* Removed pin on pytest 5.3.4. Still shouldn't use that specific version, but pytest is up to 6.0 now, so I see no need to keep a pin to avoid a .

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f057044d3566`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.