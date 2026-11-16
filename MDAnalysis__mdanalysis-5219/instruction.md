# Issue

**Title:** ENH: tpx 138 (GROMACS 2026.0) support

## Description

* Add `.tpr` parsing support for GROMACS `2026.0`, which was recently released:
https://manual.gromacs.org/2026.0/release-notes/2026/major/highlights.html

* I believe this is the first time we bump the `tpx` version support since we added support for parsing positions and velocities from `.tpr` files, so we may want to be particularly careful here.

@al42and I just used `gmx convert-tpr` with GROMACS 2026.0 on your alanine dipeptide NN potential `.tpr` provided back in gh-4919. Will that suffice to test out the new `tpr` "bells and whistles" from GROMACS 2026? The release highlights mention "Expanded support for running simulations with Neural Network Potential models, now including link atom treatment for NNP/MM, pairlist input, and electrostatic embedding models." But for the most part the `.tpr` parsing doesn't seem to need any new shims with the `.tpr` files we're currently using.

## LLM / AI generated code disclosure
LLMs or other AI-powered tools (beyond simple IDE use cases) were used in this contribution: No

## PR Checklist
<!-- Please use the following checklist to ensure the PR is ready to be reviewed/merged. -->
 - [ ] Issue raised/referenced?
 - [x] Tests updated/added?
 - [x] Documentation updated/added?
 - [x] `package/CHANGELOG` file updated?
 - [x] Is your name in `package/AUTHORS`? (If it is not, add it!)
 - [x] LLM/AI disclosure was updated.

## Developers Certificate of Origin
<!-- 
In order for this PR to be merged you **must certify that you are able to submit your code to be included in MDAnalysis**.
-->

I certify that I can submit this code contribution as described in the [**Developer Certificate of Origin**](https://developercertificate.org/), under the MDAnalysis [LICENSE](https://github.com/MDAnalysis/mdanalysis/blob/develop/LICENSE).


<!-- readthedocs-preview mdanalysis start -->
----
📚 Documentation preview 📚: https://mdanalysis--5219.org.readthedocs.build/en/5219/

<!-- readthedocs-preview mdanalysis end -->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `24548e6c55e3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.