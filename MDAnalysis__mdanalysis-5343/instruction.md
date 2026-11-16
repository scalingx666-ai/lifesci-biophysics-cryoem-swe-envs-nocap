# Issue

**Title:** Fix bond guessing with mixed-case atomtypes

## Description

Changes made in this Pull Request:
- Atomtypes are converted to uppercase for comparison with the vdwradii table (which is all uppercase)

## LLM / AI generated code disclosure
LLMs or other AI-powered tools (beyond simple IDE use cases) were used in this contribution: no

## PR Checklist
 - [x] Issue raised/referenced?
 - [x] Tests updated/added?
 - [ ] Documentation updated/added?
 - [x] `package/CHANGELOG` file updated?
 - [x] Is your name in `package/AUTHORS`? (If it is not, add it!)
 - [x] LLM/AI disclosure was updated.

## Developers Certificate of Origin

I certify that I can submit this code contribution as described in the [**Developer Certificate of Origin**](https://developercertificate.org/), under the MDAnalysis [LICENSE](https://github.com/MDAnalysis/mdanalysis/blob/develop/LICENSE).


<!-- readthedocs-preview mdanalysis start -->
----
📚 Documentation preview 📚: https://mdanalysis--5343.org.readthedocs.build/en/5343/

<!-- readthedocs-preview mdanalysis end -->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d150bfe5e3a5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.