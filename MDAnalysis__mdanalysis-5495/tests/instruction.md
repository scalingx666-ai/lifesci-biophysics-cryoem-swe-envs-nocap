# Issue

**Title:** Fix: #5494: Incorrect reduction in multiprocessing eigenvectors

## Description

Previously the `results.eigenvectors` were being reduced from multiple workers with `np.hstack`. This meant that the results from multiple time frames were being incorrectly reduced horizontally instead of stacked verically for multiple frames per worker. This was silently happening if the number of frames was evenly split between workers, but failed if the split was uneven and the frames were unable to be horizontally aggregated because of the different number of rows in their returned results.

Changes made in this Pull Request:
Change the aggregator to properly use `np.vstack` instead of `np.hstack`.

## LLM / AI generated code disclosure
LLMs or other AI-powered tools (beyond simple IDE use cases) were used in this contribution: no

## PR Checklist
<!-- Please use the following checklist to ensure the PR is ready to be reviewed/merged. -->
 - [x] Issue raised/referenced?
 - [x] Tests updated/added?
 - [x] Documentation updated/added?
 - [x] `package/CHANGELOG` file updated?
 - [x] Is your name in `package/AUTHORS`? (If it is not, add it!)
 - [x] I have read and understand the current [AI Policy](https://github.com/MDAnalysis/mdanalysis/blob/develop/AI_POLICY.md)
 - [x] LLM/AI disclosure was updated.

## Developers Certificate of Origin
<!--
In order for this PR to be merged you **must certify that you are able to submit your code to be included in MDAnalysis**.
-->

I certify that I can submit this code contribution as described in the [**Developer Certificate of Origin**](https://developercertificate.org/), under the MDAnalysis [LICENSE](https://github.com/MDAnalysis/mdanalysis/blob/develop/LICENSE).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e4884fa794f0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.