# Issue

**Title:** Add ASV benchmark for XYZ reader

## Description

<!-- 
Insert issue number that this PR fixes (if any) just after 'Fixes #'.
If this PR does not fix an existing issue, consider opening one or remove 'Fixes #' from the PR description.
-->
Contributes to #1023 

Changes made in this Pull Request:
<!-- Describe the changes that this PR makes. If applicable, use the following bullet list. --> 
- added `XYZReader` to `traj_dict` in `benchmarks/benchmarks/traj_reader.py
- updated `package/CHANGELOG` with a note under Enhancements
- results from local tests 
   
<img width="799" height="246" alt="Screenshot 2026-07-18 at 5 15 16 PM" src="https://github.com/user-attachments/assets/e9b3e41e-da16-42f0-a229-fd00af817de3" />

<img width="676" height="230" alt="Screenshot 2026-07-18 at 5 15 40 PM" src="https://github.com/user-attachments/assets/89b7d7b1-70ce-4845-8719-55039d2153b0" />



## LLM / AI generated code disclosure
<!-- Please update this disclosure to reflect if you did or did not use LLMs / AI to generate code -->
LLMs or other AI-powered tools (beyond simple IDE use cases) were used in this contribution: no

## PR Checklist
<!-- Please use the following checklist to ensure the PR is ready to be reviewed/merged. -->
 - [x] Issue raised/referenced?
 - [ ] Tests updated/added?
 - [ ] Documentation updated/added?
 - [x] `package/CHANGELOG` file updated?
 - [x] Is your name in `package/AUTHORS`? (If it is not, add it!)
 - [x] LLM/AI disclosure was updated.

## Developers Certificate of Origin
<!-- 
In order for this PR to be merged you **must certify that you are able to submit your code to be included in MDAnalysis**.
-->

I certify that I can submit this code contribution as described in the [**Developer Certificate of Origin**](https://developercertificate.org/), under the MDAnalysis [LICENSE](https://github.com/MDAnalysis/mdanalysis/blob/develop/LICENSE).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0803d8801a3d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.