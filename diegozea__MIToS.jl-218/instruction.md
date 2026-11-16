# Issue

**Title:** Preserve custom named-matrix parsers for MSA output

## Description

Custom formats that implement `parse_file` for `NamedResidueMatrix{Matrix{Residue}}` could also be read as `MultipleSequenceAlignment` in v3.10.1. The parser refactor bypassed that public method and called `_load_sequences`, causing a `MethodError` for formats without that private helper.

Restore delegation to the public named-matrix parser before wrapping its result. Select this behavior in `_parse_msa` so custom `parse_file` methods continue to take precedence without introducing dispatch ambiguities. Add a regression test covering strings, open streams and `read_file`, with gap-column removal enabled and disabled.

Validation:
- Confirmed the regression test fails on master before the fix.
- All 1,128 focused parser, annotation, I/O, writer and iterator checks passed twice on Julia 1.13, including download checks; no failures or skips.
- Ran JuliaFormatter on both changed files and `git diff --check`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `db4f4de37efe`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.