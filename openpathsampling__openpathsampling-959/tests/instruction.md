# Issue

**Title:** Switch to GitHub Actions

## Description

This disables support for Travis CI and switches to GitHub actions. [Travis CI made significant changes](https://blog.travis-ci.com/2020-11-02-travis-ci-new-billing) to its pricing plan that caused major havoc among open source projects. [Although Travis CI has reaffirmed their support of open source](https://blog.travis-ci.com/oss-announcement), their roll-out of the new model was an absolute mess (no warning, understaffed to deal with thousands of requests from OSS developers for more minutes). See [this thread on the Travis CI forums](https://travis-ci.community/t/org-com-migration-unexpectedly-comes-with-a-plan-change-for-oss-what-exactly-is-the-new-deal/10567) for some sense of the confusion. During the early uncertainty, I (and thousands of other devs) switched away from Travis. All of my projects other than OPS are now running on GitHub Actions, and so I'm moving OPS over to Actions as well.

The GitHub Actions workflows here have been tested in a separate branch, so they work. For an example, see tests on d7ee961.

To-do: Add support for building/deploying docs. That should come within a week or two (need to get some permissions, but in principle everything should be figured out).

For discussion: GitHub Actions may bring some more flexibility than we had with Travis. A few things I'm looking to try out:

* Different behaviors if a PR is marked as a draft. For example, don't run notebook tests in draft PRs (encouraging faster development cycle then).
* Separate testing aspects into different workflows: Perhaps move the notebook tests to run in parallel with unit tests. Perhaps have the docs build/deploy as a separate workflow that only happens after merging a PR.

On these points, I'm curious what contributors think. In particular pinging @sroet  and @hejung for any thoughts.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3a681c17e195`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.