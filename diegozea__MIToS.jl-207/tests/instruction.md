# Issue

**Title:** Fix SIFTS benchmark XML document lifetime

## Description

The SIFTS residue constructor benchmark stores an XML child node and then frees its owning document before any benchmark samples run. Accessing that dangling node can segfault in `xmlGetProp`; this crashed the benchmark checks for #206 twice.

Create the XML document and extract residue details in each sample's setup, then free the document in teardown. XML parsing, detail extraction, and cleanup remain outside the measured constructor. This follows [LightXML's ownership rules](https://github.com/JuliaIO/LightXML.jl#read-an-xml-file) and [BenchmarkTools' setup/teardown model](https://juliaci.github.io/BenchmarkTools.jl/stable/manual/#Setup-and-teardown-phases).

Validation: all three SIFTS residue benchmarks completed three runs of 50 samples with 10 evaluations per sample on Julia 1.13.0 / BenchmarkTools 1.8.0, with garbage collection between runs. Ran JuliaFormatter 2.14.0; `git diff --check` passes. This PR changes only `benchmark/SIFTS/Residue.jl`.

This is a prerequisite for reliable benchmark CI on #206. The action always loads benchmark definitions from `master` (`--bench-on=master`), so its current check still uses the faulty fixture until this fix is merged. The same fixture repair is already included as a separate commit in #206; merging this isolated fix first allows #206 to be checked against the repaired benchmark definitions without merging the Stockholm changes.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3e022c2c393c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.