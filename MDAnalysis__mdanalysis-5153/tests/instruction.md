# Issue

**Title:** Einstein msd verbose fix

## Description

Changes made in this Pull Request:
- Used the ProgressBar from base
- Just changed the tqdm with ProgressBar() 
- Given lagtimes and verbose in `_conclude_simple` ; range(self.n_particles) and verbose in `_conclude_fft`

Hi @orbeckst , please see the PR and tell me if I need to update or change anything.

## PR Checklist
 - [x] Issue raised/referenced?
 - [ ] Tests updated/added?
 - [ ] Documentation updated/added?
 - [x] `package/CHANGELOG` file updated?
 - [x] Is your name in `package/AUTHORS`? (If it is not, add it!)

## Developers Certificate of Origin

I certify that I can submit this code contribution as described in the [**Developer Certificate of Origin**](https://developercertificate.org/), under the MDAnalysis [LICENSE](https://github.com/MDAnalysis/mdanalysis/blob/develop/LICENSE).


<!-- readthedocs-preview mdanalysis start -->
----
📚 Documentation preview 📚: https://mdanalysis--5153.org.readthedocs.build/en/5153/

<!-- readthedocs-preview mdanalysis end -->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6b51eac50a24`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.