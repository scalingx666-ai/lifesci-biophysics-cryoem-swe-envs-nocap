# Issue

**Title:** Fix: Incorrect `capped_distance` in some triclinic box situations

## Description

In triclinic boxes where both `b_x` and `c_y` are non-zero (e.g. truncated octahedra), the `nsgrid` and `pkdtree` methods of `capped_distance` / `self_capped_distance` miss pairs within the cutoff. This affects bond guessing, selections, etc. Reproduced with the issue's [`struc.txt`](https://github.com/user-attachments/files/18682239/struc.txt) and the ; both now match `bruteforce`.

Changes made in this Pull Request:
- `_triclinic_pbc`: fix the a-axis wrapping bound, which could leave coordinates just outside the primary image (affects `apply_PBC`, `pkdtree` and `nsgrid`)
- `FastNS.coord2cellxyz`: fix the same missing term in the x cell index ()
- `FastNS._prepare_box`: size grid cells from the box's perpendicular widths, so cells are never thinner than the cutoff
- Add regression tests for `apply_PBC`, `capped_distance`, `self_capped_distance` and `FastNS` in skewed boxes

## LLM / AI generated code disclosure
LLMs or other AI-powered tools (beyond simple IDE use cases) were used in this contribution: no

I did use Claude to scan through and track down the issue, but I wrote the code and the tests.

## PR Checklist
 - [x] Issue raised/referenced?
 - [x] Tests updated/added?
 - [x] Documentation updated/added?
 - [x] `package/CHANGELOG` file updated?
 - [x] Is your name in `package/AUTHORS`? (If it is not, add it!)
 - [x] LLM/AI disclosure was updated.

## Developers Certificate of Origin

I certify that I can submit this code contribution as described in the [**Developer Certificate of Origin**](https://developercertificate.org/), under the MDAnalysis [LICENSE](https://github.com/MDAnalysis/mdanalysis/blob/develop/LICENSE).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4ba8fe755125`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.