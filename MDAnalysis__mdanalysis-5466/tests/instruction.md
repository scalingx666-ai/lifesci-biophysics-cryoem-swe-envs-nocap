# Issue

**Title:** ENH: support reading box vectors from TPR files

## Description

Changes made in this Pull Request:
- Modified `TPRReader._read_first_frame()` in `package/MDAnalysis/coordinates/TPR.py` to capture the `Box` tuple from `extract_box_info()` and set `ts.dimensions = triclinic_box(*box_info.size)`.
- Added unit conversion (`convert_units=True` converts nm to Å; `convert_units=False` preserves nm) following MDAnalysis coordinate reader standards.
- Added comprehensive unit tests in `testsuite/MDAnalysisTests/coordinates/test_tpr.py` for single (`mda.Universe(tpr)`) and double (`mda.Universe(tpr, tpr)`) TPR universes across different GROMACS TPR versions.
- Updated `package/CHANGELOG`.

## LLM / AI generated code disclosure
LLMs or other AI-powered tools (beyond simple IDE use cases) were used in this contribution: No

## PR Checklist
 - [x] Issue raised/referenced?
 - [x] Tests updated/added?
 - [x] Documentation updated/added?
 - [x] `package/CHANGELOG` file updated?
 - [x] Is your name in `package/AUTHORS`? (If it is not, add it!)
 - [x] I have read and understand the current [AI Policy](https://github.com/MDAnalysis/mdanalysis/blob/develop/AI_POLICY.md)
 - [x] LLM/AI disclosure was updated.

## Developers Certificate of Origin

I certify that I can submit this code contribution as described in the [**Developer Certificate of Origin**](https://developercertificate.org/), under the MDAnalysis [LICENSE](https://github.com/MDAnalysis/mdanalysis/blob/develop/LICENSE).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4f6d54a9e9b7`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.