# Issue

**Title:** Write multiple Stockholm and Clustal alignments

## Description

`write_file` and `print_file` now accept typed collections and iterators of alignments for Stockholm and Clustal. Each alignment is written as a complete record using its existing format writer, without collecting the input or asking for its length.

```julia
write_file("alignments.sto", [msa1, msa2], Stockholm)

eachmsa(
    "Pfam-A.seed.gz",
    Stockholm;
    generatemapping = true,
    useidcoordinates = true,
) do msas
    write_file("Pfam-A.seed.aligned.stockholm.gz", msas, Stockholm)
end
```

Inputs must declare an element type compatible with `AbstractMatrix{Residue}`. Unknown element types and incompatible types, including `Any`, raise short `ArgumentError` messages before the destination is opened. The same validation is used for direct stream output. Single-alignment writing and the existing file-mode argument remain available; Clustal keywords such as `showcounts` are forwarded by collection `print_file` calls.

Output uses one stream, including for gzip. Empty typed collections produce empty output, with valid gzip encoding when requested. Output is closed on iteration errors, and the caller retains responsibility for any input stream. The Pfam documentation example saves the original column and residue mappings before removing insert columns, then shows how to reuse those mappings when rereading the output with the default options.

### Validation

- JuliaFormatter 2 applied to every edited Julia file.
- On Julia 1.13, focused utility, MSA, PDB/PDBML and SIFTS I/O checks passed twice, including real network downloads. All 157 writer checks passed twice.
- Writer tests cover both formats, plain and gzip output, all four alignment representations, annotations, `eachmsa`, empty input, append mode, keyword forwarding, and writing each record before requesting the next. Invalid element types preserve existing destinations, and iteration failures still close compressed output correctly.
- The Pfam example is tested with two alignments from the existing small fixture: original column and residue numbers survive insert removal and gzip output, filtered annotations are retained, and a second write/read cycle preserves all annotations without regenerating mappings.
- Manual test using the current EBI `Pfam-A.seed.gz` URL: all 30,134 families (1,646,118 sequences) preserved their sequences, names, column names and all annotations through two gzip write/read cycles. This large-data check is outside the test suite.
- The existing probability-plot example now retrieves UniProt P29374 through EMBL-EBI DBfetch: the previous URL returned HTTP 503 and prevented the documentation build. The replacement returns the identical 1,257-residue sequence, and its probability calculation passed locally.
- GitHub CI is green on the latest commit: Julia 1.9 and latest Julia on Linux, Windows and macOS; documentation; formatting; benchmarks; and coverage checks.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3901f0c8d985`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.