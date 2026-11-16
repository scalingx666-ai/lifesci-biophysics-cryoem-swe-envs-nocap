# Issue

**Title:** Small miscellaneous patches + features

## Description

**Summary**

1) Patched restarts and OMP compatibility of pair tracker ()
2) Clarifies whether D2min calculation in fix nonaffine/displacement is intensive/extensive and lets user swap definitions
3) Cleaning up some long comments in the code/documentation relating to classic gran tangential models
4) add some tweaks to unittest YAML files to make the tests pass or be skipped on ARM64

**Related Issue(s)**



**Author(s)**

Joel Clemmer (SNL)

**Licensing**

By submitting this pull request, I agree, that my contribution will be included in LAMMPS and redistributed under either the GNU General Public License version 2 (GPL v2) or the GNU Lesser General Public License version 2.1 (LGPL v2.1).

**Artificial Intelligence (AI) Tools Usage**

By submitting this pull request, I confirm that I did NOT use any AI tools to generate
all or parts of the code and modifications in this pull request.

**Backward Compatibility**

Old restart files may not work with pair tracker

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c029915e1d26`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.