# Issue

**Title:** feat: Add support for reading unit cell dimensions from MOL2 CRYSIN records

## Description

Changes made in this Pull Request:

Implemented logic in MOL2Parser (package/MDAnalysis/coordinates/MOL2.py) to parse @<TRIPOS>CRYSIN records.

Extracted unit cell dimensions (lengths and angles) from the CRYSIN block and assigned them to ts.dimensions.

Added a new test data file with a CRYSIN record: testsuite/MDAnalysisTests/data/mol2/test_crysin.mol2.

Registered the new test file in testsuite/MDAnalysisTests/datafiles.py.

Added a regression test test_mol2_crysin_dimensions in testsuite/MDAnalysisTests/coordinates/test_mol2.py to verify dimensions are loaded correctly.

PR Checklist
[x] Issue raised/referenced?

[x] Tests updated/added?

[ ] Documentation updated/added?

[x] package/CHANGELOG file updated?

[x] Is your name in package/AUTHORS? (If it is not, add it!)

Developers Certificate of Origin
I certify that I can submit this code contribution as described in the [Developer Certificate of Origin](https://developercertificate.org/), under the MDAnalysis [LICENSE](https://github.com/MDAnalysis/mdanalysis/blob/develop/LICENSE).


<!-- readthedocs-preview mdanalysis start -->
----
 Documentation preview : https://mdanalysis--5187.org.readthedocs.build/en/5187/

<!-- readthedocs-preview mdanalysis end -->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6a1befef3601`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.