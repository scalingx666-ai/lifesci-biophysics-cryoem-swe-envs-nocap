# Issue

**Title:** Add lazy iteration over Stockholm and Clustal alignments

## Description

.

Add `eachmsa(source, format::Type[, output::Type]; kwargs...)` to read Stockholm and Clustal files one alignment at a time. The example in #202 yields separate four- and three-sequence MSAs. A shared `MSAIterator` manages the stream, output type, parsing options, and cleanup; format-specific header patterns and parsing methods identify and read each alignment. Header scanning is shared with the check used by `read_file` to warn about additional alignments. Both `eachmsa` calling forms restrict their format argument to `Stockholm` and `Clustal`; unsupported formats raise `MethodError` before file access.

Both compressed and uncompressed files use the same interface. URL detection, temporary download filenames and gzip wrapping are shared through private helpers in `Utils`. Iterator closure and constructor error handling use the same cleanup helper. Gzip input is read in one pass, and URLs are downloaded once per iterator. The documented `do` form closes the stream and removes temporary downloads even after an early exit or an exception.

Clustal iteration reuses MIToS's parser, reading lines up to the next CLUSTAL header without buffering the whole alignment text. Wrapped blocks, residue counts, and conservation annotations remain within their alignment. The Stockholm and Clustal parsers return `has_next` alongside the loaded data. Clustal returns whether it consumed another header; Stockholm returns `nothing` and leaves that check to its caller. The flag is used by Clustal iteration and by the warning in `read_file`. A shared loader selects whether to request annotations from the output type. Shared MSA construction methods preserve the output types and parsing options of `parse_file`.

For concatenated Clustal input, `read_file` and `parse_file` return only the first alignment. A new CLUSTAL header ends that alignment; subsequent records no longer append to its sequences or conservation annotations.

For both formats, `read_file` warns when another alignment header is found: “Read only the first alignment; use `eachmsa` to read all.” The check stops at that header and works with gzip files. `parse_file` and `eachmsa` remain quiet, and single-alignment files with trailing whitespace produce no warning.

### Clustal format references

- [MEME Suite's ClustalW specification](https://meme-suite.org/meme/doc/clustalw-format.html) describes a header followed by sequence blocks separated by blank lines. A blank line therefore does not mark a new alignment.
- [Biopython's official AlignIO documentation](https://biopython.org/docs/latest/api/Bio.AlignIO.html#input) documents iteration over multiple alignments and concatenated alignment files.
- [Biopython's Clustal reader](https://github.com/biopython/biopython/blob/57056bbfc48246f743315f52da772848c9bd0bfa/Bio/AlignIO/ClustalIO.py#L194-L209) explicitly recognizes concatenated alignments and retains the next header. This provides a concrete interoperability reference for treating complete Clustal alignments, each with its own header, as separate records.

These sources informed the format behavior. The implementation builds on MIToS's existing code; no Biopython code was copied, and the MIT license is unchanged.

### Validation

On Julia 1.13, all 701 focused utility, iterator and MSA I/O checks passed twice, including real URL downloads, 378 iterator checks and 274 MSA I/O checks. They cover both formats, gzip, all four output types, lazy parsing, early exits, errors, annotations, lookahead, and the warning about additional alignments. A stream-position check verifies that `parse_file` leaves the next Stockholm header unread.

The tests cover all output types while checking shared opening and cleanup behavior once per format and compression option. Warning cases are grouped together, and successive `parse_file` calls have their own testset. Tests verify that unsupported formats have no applicable method for either calling form and raise `MethodError`, including calls with parsing keywords. Existing annotation tests cover the column-mapping getter.

URL tests use actual downloads of existing GitHub fixtures, including a gzip file, and check temporary-file cleanup after exhaustion or early closure. Download tests skip only proxy/DNS, connection, or timeout failures. HTTP errors and parsing failures are not skipped.

JuliaFormatter 2 ran on all edited Julia files. CI runs the full package suite and documentation checks.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0ce717038b64`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.