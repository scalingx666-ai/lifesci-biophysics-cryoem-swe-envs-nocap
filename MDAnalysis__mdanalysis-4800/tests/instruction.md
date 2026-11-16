# Issue

**Title:** Transfer of `fasteners` dependency to `filelock`

## Description

Changes made in this Pull Request:
 - Removal of `fasteners` dependency from `XDR.py` and all installation files
 - Addition of `filelock` as an replacement  dependency of `fasteners`


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
📚 Documentation preview 📚: https://mdanalysis--4800.org.readthedocs.build/en/4800/

<!-- readthedocs-preview mdanalysis end -->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `bdfb2c9f9a91`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.