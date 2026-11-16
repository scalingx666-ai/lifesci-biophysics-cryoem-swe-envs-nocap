# Issue

**Title:** Run integration tests nightly rather than on every pull request

## Description

Addresses the structural half of #1135.

The integration tests reach CATH, ECOD, RCSB, EBI and others. That is what makes them worth having — they are how we learn that an upstream service changed a URL, a format or a redirect. It is also what makes them a poor gate on pull requests, because any of those services having a bad day blocks every contributor, and a real regression then cannot be distinguished from the surrounding noise.

That is not hypothetical. Every `PR Build` since 2025-12-19 has failed, on all five matrix jobs, whatever the pull request contained — a one-line dependency bump (#1132) fails exactly as a feature branch does. Two integration tests account for it: `CathDomainTest`, because `download.cathdb.info` began redirecting http to https (#1138), and `EcodInstallationTest.testVersion`, because ECOD redesigned its distribution format (#1139).

`Master Build` already excludes the module with `-pl '!biojava-integrationtest'`. This applies the same exclusion to pull requests and adds a scheduled workflow that runs the full suite nightly, so the coverage is kept but is no longer in anybody's way. An upstream break still gets caught, within a day, by a run whose failure means what it says.

### Both failures are now fixed, which changes the argument for this PR

#1133 fixes CATH and #1141 fixes ECOD, so with those merged `PR Build` should be green **with** the integration tests still running. The emergency case for this PR is therefore gone, and I would rather say so than let it merge on momentum.

What is left is the stronger argument, and it is the one worth weighing:

- **Both breaks were upstream changes, and both took months to notice.** ECOD changed its file format at v294.1. The signal we got was `EcodInstallationTest.testVersion:285 Values should be different. Actual: latest`, on somebody else's unrelated pull request, competing for attention with a second failure and with eight months of identical red checks. It named neither ECOD nor the format. A nightly run would have reported it the following morning, alone, against a tree nobody had just changed.
- **Meanwhile the cost is paid by everyone.** `ecod.latest.domains.txt` is 657 MB, pulled once per matrix job, five times per pull request. That is not why the test failed —  — but it is real, and it buys coverage that a one-second offline test now provides better (#1141 adds fixtures pinning every ECOD file layout the project has ever read).

So the trade is not "keep the build green" any more. It is: should a pull request's check depend on five third-party services being up and fast, when the thing those services actually tell us arrives more usefully as a nightly failure?

### The nightly job

- **One JDK (21).** The pull request build already covers 11, 17 and 21; the point of this run is the network paths, not the language level.
- **90 minute timeout.** These downloads are large and GitHub's six hour default is not a useful ceiling if something hangs.
- **Surefire reports kept 30 days on failure.** GitHub expires run logs after 90 days, and those reports carry the stack traces identifying which service misbehaved — exactly what was missing when diagnosing the current breakage, where the logs for the older failures had already expired.
- **`workflow_dispatch`**, so it can be run by hand to confirm a service is back.

### What this gives up

Plainly: a pull request that breaks an integration test is not caught until that night. For changes touching the download or parsing paths, that is a real regression window. If that trade is unacceptable — and now that both tests are fixed it is a much more open question than when I opened this — the coherent response is to close this PR. I would rather that than have it merged by default.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5b29945954ea`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.