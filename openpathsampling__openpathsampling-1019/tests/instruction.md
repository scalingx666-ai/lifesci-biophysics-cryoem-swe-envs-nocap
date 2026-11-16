# Issue

**Title:** Test against OpenMM RCs

## Description

Every time I see an OpenMM release candidate announced, I think, "I should really try that out and run the OPS test suite against it." But I don't. And this time we got hit by a bug () that could have been fixed in prerelease had I run those tests.

So the other day I created a GitHub Action that checks for recent prereleases of a conda package. This PR is to implement use of it here. I think I'll need to merge this PR before I can actually check that everything works, because the workflow that gets triggered to run the tests won't exist until it has been merged to master. So this may require a follow-up PR to complete everything, although the basic idea seems to be working fine in https://github.com/dwhswenson/conda-rc-check/.

Overall approach I plan to take: Every other day we check to see if an RC has been released within the last 3 days. If so, we trigger a workflow that will test against that RC. This is a simplified version of our test suite, only testing against the current default Python version from miniconda, and using the OpenMM RC. (The 2 day/3 day difference is in case GitHub's scheduler doesn't run at exactly the same time, so this ensures some overlap in the time windows.)

For now I'm using the `main` branch of my Action, but I'll release a 1.0 of that once I've tried it out here.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `58906040830d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.