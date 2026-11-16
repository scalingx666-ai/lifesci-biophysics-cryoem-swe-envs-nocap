# Issue

**Title:** Prepare v3.11.0 release

## Description

Prepare MIToS v3.11.0 with release notes covering master through #218, relative to v3.10.1. The minor version reflects support for reading and writing files containing multiple sequence alignments. The PR changes only `NEWS.md` and the version in `Project.toml` compared with master.

The notes describe:

- Reading Stockholm and Clustal files one alignment at a time with `eachmsa`, and writing alignment collections or iterators (#208, #209).
- Extending `eachmsa` through `support_eachmsa`, `hasnextmsa` and `parse_file`, documented in the new Development section (#211).
- Using public parsing and printing methods for custom alignment types, while preserving custom named-matrix parsers when requesting `MultipleSequenceAlignment` (#211, #218).
- Keeping Clustal alignments separate even with indented headers, accepting indented Stockholm headers, and warning when `read_file` reads only the first alignment (#208, #211, #217).
- Writing the Stockholm header and separating fields with spaces for compatibility with other readers (#206).
- Fixing the SIFTS benchmark's XML document lifetime (#207).

The branch includes current master (`7911229e`), including the contributor-guidance update (#216). The agreed introductory paragraph is retained.

Validation:

- Compared the notes with the merged changes since v3.10.1.
- Verified that the PR changes only the new NEWS section and the version relative to master; all other project metadata and earlier release notes are unchanged.
- Parsed the release notes as Markdown and loaded MIToS as v3.11.0 on Julia 1.13.0.
- `git diff --check` passed. CI for this updated revision is pending.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7911229e5886`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.