# Issue

**Title:** install: requests/urllib supports brotli

## Description

Somehow I missed that `urllib3/requests` does now support `brotli`, through the optional install parameter in urllib3 (and by using latest releases from both).  Discovered this yesterday when I had a different version of requests installed in a different virtual environment and was running into problem decoding brotli encoded data (because it was trying to decode an already decoded response, and I had brotli installed in that environment).

A few things to keep an eye out for w/ this PR:

1. This "unpins" and upgrades `urlib3`.  Also upgrades requests, but that wasn't unpinned, so don't think it's a big deal.  Not sure if pinning urlib3 was important, can repin if necessary.  Brotli was added in 1.25 of urlib3.  I tested on 1.25.7.  Requests added support in latest release, 2.22.0.

2. I'm not sure what this does for people who install from source.  I think it will be fine though, as long as they reinstall from requirements.txt

3. I didn't/can't run the entire test suite

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `87d51977cce7`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.