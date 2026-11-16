# Issue

**Title:** Removing mutable data structures and function calls as default arguments in the entire codebase

## Description

Changes made in this Pull Request:
 - Removing mutable data structures and function calls as default arguments in the entire codebase and initializing/calling them inside the function instead
 - There were 12 issues found using the ruff module(select = B006, B008 and ignore=E501) which were all fixed


PR Checklist
------------
 - [x] Tests
 - [x] CHANGELOG updated?
 - [x] Issue raised/referenced

## Developers certificate of origin
- [x] I certify that this contribution is covered by the LGPLv2.1+ license as defined in our [LICENSE](https://github.com/MDAnalysis/mdanalysis/blob/develop/LICENSE) and adheres to the [**Developer Certificate of Origin**](https://developercertificate.org/).


<!-- readthedocs-preview mdanalysis start -->
----
📚 Documentation preview 📚: https://mdanalysis--4834.org.readthedocs.build/en/4834/

<!-- readthedocs-preview mdanalysis end -->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f4e0f0b69541`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.