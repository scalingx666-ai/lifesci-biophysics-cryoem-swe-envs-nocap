# Issue

**Title:** Recognize indented Clustal alignment headers

## Description

A Clustal header with leading spaces or tabs was accepted by `hasnextmsa` but missed by the parser. Consecutive alignments with the same sequence identifier could therefore be concatenated into one, and `read_file` would not warn that more alignments were present.

Use the same whitespace normalization for detecting headers and blank lines, while keeping the original line available for `unread`. Sequence and conservation-column spacing remain intact.

Regression tests cover space- and tab-indented headers, plain and gzip files, the `read_file` warning, and exact preservation of the next buffered header. All eight new assertions failed against master before the fix.

Validation:

- All 1,040 focused reader, writer and I/O checks passed twice on Julia 1.13, with no failures or skips, including the existing download tests.
- JuliaFormatter applied; `git diff --check` passed.
- All six Julia/platform test jobs, documentation, formatting, benchmarks, both Codecov checks and all six Coveralls platform checks passed for `514b1c59`. Codecov reports 100% coverage of changed executable lines.
- : Clustal reading is 21.9 ± 1.7 μs on master versus 22.3 ± 1.7 μs on this PR, within measurement variation, with the same 429 allocations and 41.2 kB. FASTA writing is also within measurement variation, with unchanged allocations.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f9f24954fe63`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.