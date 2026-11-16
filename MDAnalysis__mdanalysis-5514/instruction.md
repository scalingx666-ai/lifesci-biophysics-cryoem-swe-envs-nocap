# Issue

**Title:** ENH: Add 'chain' as alias for 'chainID' in selections

## Description

Most molecular selection syntax in other programs uses `chain` for specifying the chains. This PR just adds `chain` as an alias for our existing `chainID` so there is less barrier to entry.

Changes made in this Pull Request:
- Adds `chain` as an alias for `chainID` in selection strings

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

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ae9ff4b73860`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.