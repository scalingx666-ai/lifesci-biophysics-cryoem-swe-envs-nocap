# Issue

**Title:** change XTC/TRR tests for lock file presence

## Description

Changes made in this Pull Request:
- filelock >= 3.29.5 will now never remove the lockfile (avoids a race condition on POSIX): changed the correspond test (xfail for older versions of filelock)
- filelock also apparently changed behavior on Windows: it used to leave the lock file around but now it seems to be removing it (based on the windows runners): changed the test to account for this new behavior 
- updated doc string XDRBaseReader._load_offsets to indicate that we use filelock
- added more docs to XDR page (more details on offsets)
- add filelock docs to intersphinx mapping


## LLM / AI generated code disclosure

LLMs or other AI-powered tools (beyond simple IDE use cases) were used in this contribution: no

## PR Checklist
<!-- Please use the following checklist to ensure the PR is ready to be reviewed/merged. -->
 - [x] Issue raised/referenced?
 - [x] Tests updated/added?
 - [x] Documentation updated/added?
 - [x] `package/CHANGELOG` file updated?
 - [x] Is your name in `package/AUTHORS`? (If it is not, add it!)
 - [x] LLM/AI disclosure was updated.

## Developers Certificate of Origin
<!-- 
In order for this PR to be merged you **must certify that you are able to submit your code to be included in MDAnalysis**.
-->

I certify that I can submit this code contribution as described in the [**Developer Certificate of Origin**](https://developercertificate.org/), under the MDAnalysis [LICENSE](https://github.com/MDAnalysis/mdanalysis/blob/develop/LICENSE).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `830e2af237ab`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.