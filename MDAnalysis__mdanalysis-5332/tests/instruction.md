# Issue

**Title:** FIX (CI): Update First Contributor Greeting workflow inputs and add issue_message

## Description

This PR fixes the `First Time Contributor Greetings` workflow as observed in PR #5325 , PR #5331 and similar other PRs from new contributors, which was failing due to parameter mismatch with `actions/first-interaction@v3` action. The action now requires inputs to be in snake_case `(repo_token`, `pr_message`) and treats `issue_message` as a mandatory field

<!-- 
Insert issue number that this PR fixes (if any) just after 'Fixes #'.
If this PR does not fix an existing issue, consider opening one or remove 'Fixes #' from the PR description.
-->


Changes made in this Pull Request:
<!-- Describe the changes that this PR makes. If applicable, use the following bullet list. --> 
- Updated `repo-token` to `repo_token` and `pr-message` to `pr_message` in `.github/workflows/greetings.yml.`
- Added a standard `issue_message` to satisfy the action's requirements and welcome first-time issue creators.

## LLM / AI generated code disclosure
<!-- Please update this disclosure to reflect if you did or did not use LLMs / AI to generate code -->
LLMs or other AI-powered tools (beyond simple IDE use cases) were used in this contribution: no

## PR Checklist
<!-- Please use the following checklist to ensure the PR is ready to be reviewed/merged. -->
 - [x] Issue raised/referenced? Referenced failure in PR #5325 
 - [x] Tests updated/added? N/A
 - [x] Documentation updated/added? N/A
 - [x] `package/CHANGELOG` file updated? 
 - [x] Is your name in `package/AUTHORS`? (If it is not, add it!)
 - [x] LLM/AI disclosure was updated.

## Developers Certificate of Origin
<!-- 
In order for this PR to be merged you **must certify that you are able to submit your code to be included in MDAnalysis**.
-->

I certify that I can submit this code contribution as described in the [**Developer Certificate of Origin**](https://developercertificate.org/), under the MDAnalysis [LICENSE](https://github.com/MDAnalysis/mdanalysis/blob/develop/LICENSE).


<!-- readthedocs-preview mdanalysis start -->
----
📚 Documentation preview 📚: https://mdanalysis--5332.org.readthedocs.build/en/5332/

<!-- readthedocs-preview mdanalysis end -->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2a3d7b218fec`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.