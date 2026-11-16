# Issue

**Title:** Implementation of Parallelization to `MDAnalysis.analysis.contacts`

## Description

Changes made in this Pull Request:
 - added backends and aggregators to `Contacts` in `analysis.contacts`
 - added `get_box` function in `analysis.contacts` to fix error, which appeared during parallelization
 - added the `client_Contacts` in `conftest.py`
 - added `client_Contacts` in `run()` in `test_contacts.py`

The function `get_box` was implemented due to this Error occuring in pytests with parallelization:
`AttributeError: Can't get local object 'Contacts.__init__.<locals>.<lambda>'`

PR Checklist
------------
 - [x] Tests?
 - [x] Docs?
 - [x] CHANGELOG updated?
 - [x] Issue raised/referenced?

## Developers certificate of origin
- [x] I certify that this contribution is covered by the LGPLv2.1+ license as defined in our [LICENSE](https://github.com/MDAnalysis/mdanalysis/blob/develop/LICENSE) and adheres to the [**Developer Certificate of Origin**](https://developercertificate.org/).


<!-- readthedocs-preview mdanalysis start -->
----
📚 Documentation preview 📚: https://mdanalysis--4820.org.readthedocs.build/en/4820/

<!-- readthedocs-preview mdanalysis end -->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7f686ca0daa8`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.