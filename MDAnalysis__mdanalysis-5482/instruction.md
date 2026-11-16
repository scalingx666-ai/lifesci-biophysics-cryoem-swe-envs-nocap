# Issue

**Title:** Expose computed density in InterRDF results

## Description

Changes made in this Pull Request:
- Expose computed average number density as `results.density` (and alias `results.rho`) in `InterRDF` and `InterRDF_s` when `norm` is `"rdf"` or `"density"`.
- Guard against zero-division / zero box volume cases when computing density.
- Add unit tests for exposed density in `test_rdf.py` and `test_rdf_s.py`.
- Update `package/CHANGELOG` and `package/AUTHORS`.

## LLM / AI generated code disclosure
LLMs or other AI-powered tools (beyond simple IDE use cases) were used in this contribution: no

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

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `06e82b9b3ac2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.