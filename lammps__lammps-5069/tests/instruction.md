# Issue

**Title:** Save unit test results as JUnit XML artifacts on post-merge runs

## Description

**Summary**

This is the first step of making automated test results visible on a planned test status website (aggregating regression tests, the unit test platform matrix, coverage, and static analysis) instead of requiring manual downloading and unpacking of artifacts.

The six workflows that run unit tests via CTest (Linux BIGBIG, Linux FFT_SINGLE, macOS, Windows MSVC, Linux ARM64, and the six KOKKOS host backend/precision combinations) now pass `--output-junit` to ctest and upload the resulting XML file as a workflow artifact.

The upload step only runs for `push` events, i.e. post-merge runs on the develop (and maintenance) branches. Results from pull request test runs are *not* published: those are for the submitters to deal with. The step uses `if: always()` so results are uploaded precisely when tests fail, which are the runs worth inspecting.

**Related Issue(s)**

None yet; part of a larger effort to revive the (currently manual) full regression test workflow with accessible reporting.

**Author(s)**

Axel Kohlmeyer, Temple U (akohlmey at gmail.com)

**Licensing**

By submitting this pull request, I agree, that my contribution will be included in LAMMPS and redistributed under either the GNU General Public License version 2 (GPL v2) or the GNU Lesser General Public License version 2.1 (LGPL v2.1).

**Backward Compatibility**

No impact on LAMMPS itself; CI-only change. `ctest --output-junit` requires CMake >= 3.21, which all GitHub-hosted runners provide.

**Detailed Description**

- The JUnit XML file name encodes the configuration (e.g. `junit-linux-x86_64-bigbig.xml`, `junit-kokkos-openmp-mixed.xml`) so that downstream aggregation can key results per configuration; the KOKKOS workflow derives the name from its matrix variables.
- Artifact retention uses the repository default (90 days), which is ample for a nightly ingestion job.
- Downstream consumers select runs via the Actions API filtered by `event == push` and `head_branch == develop`, so the artifacts from maintenance-branch merges are available but unused for now.

**AI Tools Usage**

The workflow edits in this pull request were written by Claude Code (Claude Fable 5) under direction of Axel Kohlmeyer, who reviewed the changes.

**Post Submission Checklist**

- [x] The pull request is complete
- [x] The source code follows the LAMMPS formatting guidelines
- [ ] The feature has been verified to work with the conventional build system (n/a, CI workflow change only)
- [ ] The feature has been verified to work with the CMake based build system (n/a, CI workflow change only)
- [ ] Suitable tests have been added to the unittest tree. (n/a)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b54f0e7b0fb0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.