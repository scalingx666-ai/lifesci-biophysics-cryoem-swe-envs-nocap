# Issue

**Title:** update Python versions gh-ci-cron.yaml

## Description

Changes made in this Pull Request:
- remove Python 3.9 from GH cron workflow
- add 3.13 to PyPi installation
- hold off on 3.13 for conda because of #4805



PR Checklist
------------
 - [ ] Tests?
 - [ ] Docs?
 - [ ] CHANGELOG updated?
 - [ ] Issue raised/referenced?

## Developers certificate of origin
- [x] I certify that this contribution is covered by the LGPLv2.1+ license as defined in our [LICENSE](https://github.com/MDAnalysis/mdanalysis/blob/develop/LICENSE) and adheres to the [**Developer Certificate of Origin**](https://developercertificate.org/).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ac145ec3892a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.