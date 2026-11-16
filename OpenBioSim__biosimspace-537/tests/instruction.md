# Issue

**Title:** Fix issue #536

## Description

This PR  by adding `pip_check: false` to the `rattler-build` recipe. The issue is ultimately down to `ambertools` and packaging inconsistencies/annoyances within the `omsf` ecosystem, but I'll fix in the easiest way that still allows us to test against the most up-to-date dependencies.

(Note that `openff-interchange-base` dropped Python 3.11 support in their 0.5.3 _patch_ release, a change that wasn't documented in the release notes. They also flipped a `pydantic` pin in the next patch release, which has caused resolution issues with other tools in the ecosystem, which haven't updated theirs.)

* I confirm that I have merged the latest version of `devel` into this branch before issuing this pull request (e.g. by running `git pull origin devel`): [y]
* I confirm that I have permission to release this code under the GPL3 license: [y]

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `64f620045b23`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.