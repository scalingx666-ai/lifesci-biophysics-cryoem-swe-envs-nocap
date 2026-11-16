# Issue

**Title:** FIX: Full resname matching for nucleic acid analysis

## Description

I had a surface level look (searching for similar code) and couldn't find any other usage of this - so it should be the only occurrence. Tests were previously passing because of the bug and have now been fixed. Could cause issues if users were relying on this bug but still worth fixing ASAP.

Changes made in this Pull Request:
- Resname matching was just chaged from `resname[0]` -> `resname` as put forward in the issue
- Some additional tests added to better capture and guard against the issue

## LLM / AI generated code disclosure
LLMs or other AI-powered tools (beyond simple IDE use cases) were used in this contribution: no

## PR Checklist
 - [x] Issue raised/referenced?
 - [x] Tests updated/added?
 - [x] Documentation updated/added?
 - [x] `package/CHANGELOG` file updated?
 - [x] Is your name in `package/AUTHORS`? (If it is not, add it!)
 - [x] LLM/AI disclosure was updated.

## Developers Certificate of Origin

I certify that I can submit this code contribution as described in the [**Developer Certificate of Origin**](https://developercertificate.org/), under the MDAnalysis [LICENSE](https://github.com/MDAnalysis/mdanalysis/blob/develop/LICENSE).


<!-- readthedocs-preview mdanalysis start -->
----
📚 Documentation preview 📚: https://mdanalysis--5365.org.readthedocs.build/en/5365/

<!-- readthedocs-preview mdanalysis end -->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2a3d7b218fec`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.