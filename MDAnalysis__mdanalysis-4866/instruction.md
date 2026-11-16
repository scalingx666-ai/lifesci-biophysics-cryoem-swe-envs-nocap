# Issue

**Title:** ENH: support tpx 134 (GMX 2024.4)

## Description

* Fixes gh-4855.

* Add support for reading topology data from `.tpr` files produced by GROMACS 2024.4, corresponding to tpx version `134`.

* My approach was similar to the one I used in gh-4523, except for two things: 1) I retrieved the current generation (`28`) from a `print` in our own binary parser rather than rebuilding GMX from source with added `printf`, and more importantly 2) I had to add some shims because the `.tpr` format has changed.

* With regard to the additional field data now stored in the `.tpr` format, I was involved in reviewing it upstream at: https://gitlab.com/gromacs/gromacs/-/merge_requests/4544. You can see that the changes related to the MARTINI functional form made it into the `v2024.4` tag i.e., here:
https://gitlab.com/gromacs/gromacs/-/blob/v2024.4/src/gromacs/gmxpreprocess/convparm.cpp?ref_type=tags#L372

* The new `.tpr` binary assets added here were produced using `gmx convert-tpr` as I did for previous adjustments of this type, and I did confirm that the tests failed before adding the source code shims in `utils.py`.

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
📚 Documentation preview 📚: https://mdanalysis--4866.org.readthedocs.build/en/4866/

<!-- readthedocs-preview mdanalysis end -->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c08cb797fd1a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.