# Issue

**Title:** Add mass to MDAnalysis base units

## Description

Changes made in this Pull Request:
- Added `mass` to the `MDAnalysis.units` base-units documentation table.
- Added `"mass": "u"` to `MDANALYSIS_BASE_UNITS`.
- Updated the corresponding base-units test.

I did not update `package/CHANGELOG` because this is a small documentation/base-units table fix.

The SI value used for `u` is `1.66053892103219 × 10^-27 kg`, which is consistent with the CODATA 2010 constants already used in `MDAnalysis.units`.

Note that the User Guide currently lists a newer value for `u` (`1.66053906660(50) × 10^-27 kg`). This PR intentionally keeps the `MDAnalysis.units` API documentation consistent with the existing CODATA 2010 values used by the module, following the maintainer guidance in the issue.

## LLM / AI generated code disclosure

LLMs or other AI-powered tools (beyond simple IDE use cases) were used in this contribution: yes

## PR Checklist
 - [x] Issue raised/referenced?
 - [x] Tests updated/added?
 - [x] Documentation updated/added?
 - [ ] `package/CHANGELOG` file updated?
 - [x] Is your name in `package/AUTHORS`? (If it is not, add it!)
 - [x] LLM/AI disclosure was updated.

## Developers Certificate of Origin

I certify that I can submit this code contribution as described in the [**Developer Certificate of Origin**](https://developercertificate.org/), under the MDAnalysis [LICENSE](https://github.com/MDAnalysis/mdanalysis/blob/develop/LICENSE).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ceb2e3e8e0ab`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.