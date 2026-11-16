# Issue

**Title:** Use Langevin integrator for free-energy simulations

## Description

I was just going through the recent Exscientia PRs and saw , so thought I'd port it to the main code base, since my config is based on theirs. Perhaps there should be an easier mechanism for this sort of thing. I can obviously compare at the point of synchronisation, but it's obviously a lot of work if there are many changes.

* I confirm that I have merged the latest version of `devel` into this branch before issuing this pull request (e.g. by running `git pull origin devel`): [y]
* I confirm that I have permission to release this code under the GPL3 license: [y]

## Suggested reviewers:
@chryswoods

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4480a5ac9612`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.