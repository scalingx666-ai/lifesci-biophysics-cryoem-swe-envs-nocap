# Issue

**Title:** Add release notes for version 1.1.17 to `coot.appdata.xml`

## Description

This is necessary for Flathub to reflect that the version has been upgraded.

---

This pull request updates the `coot.appdata.xml` file to include details about a new software release. The most notable change is the addition of version `1.1.17` with its release date and a list of bug fixes.

Release updates:

* Added a new release entry for version `1.1.17` with the release date `2025-06-14`. The update includes descriptions of four bug fixes:
  - Fixed saving symmetry coordinates.
  - Fixed color-by-B-factor coloring.
  - Fixed placing atoms at the pointer.
  - Restored optional Gemmi usage.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c699350d57e4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.