# Issue

**Title:** Fixes MDAnalysis.analysis.density.convert_density() method has an invalid default unit value

## Description

Changes made in this Pull Request:
 - Changed the default unit in the convert_density() function to `Angstrom^{-3}` from `Angstrom`
 - Added new tests to check if the changes to the code(default unit) solve the issue in the `testsuite/MDAnalysisTests/analysis/test_density.py`


Before 
```python
def convert_density(self, unit='Angstrom'):
```

After
```python
def convert_density(self, unit='Angstrom^{-3}'):
```

PR Checklist
------------
 - [x] Tests?
 - [ ] Docs?
 - [x] CHANGELOG updated?
 - [x] Issue raised/referenced?

## Developers certificate of origin
- [x] I certify that this contribution is covered by the LGPLv2.1+ license as defined in our [LICENSE](https://github.com/MDAnalysis/mdanalysis/blob/develop/LICENSE) and adheres to the [**Developer Certificate of Origin**](https://developercertificate.org/).


<!-- readthedocs-preview mdanalysis start -->
----
📚 Documentation preview 📚: https://mdanalysis--4833.org.readthedocs.build/en/4833/

<!-- readthedocs-preview mdanalysis end -->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `fa734782a94d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.