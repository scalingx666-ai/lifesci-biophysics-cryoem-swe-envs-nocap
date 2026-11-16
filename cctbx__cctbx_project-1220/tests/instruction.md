# Issue

**Title:** probe2: batch the per-dot DotScorer calls into one call per atom

## Description

`_generate_interaction_dots()` calls `DotScorer::check_dot()` once per dot, which
crosses the boost-python boundary and re-marshals the interacting and excluded
atom lists every time. On a typical NMR ensemble model that is roughly 275k calls
per model. Profiling on 2C6B put the per-dot Python loop and call overhead at
about 1.0 s/model while the geometry itself was under 0.1 us/dot.

This adds `DotScorer::check_dots()`, which loops `check_dot()` in C++ over all of
one atom's dots and returns only the results a per-dot caller would keep, dropping
`Ignore` results and non-overlapping annular dots. `CheckDotResult` gains a
`dotOffset` field so each returned result identifies its dot. probe2 now makes one
call per source atom instead of one per dot.

`check_dot()` itself is unchanged, so nothing is reimplemented and no value can
move. The only thing that changes is which results cross into Python.

### Reviewing

The `probe2.py` diff looks like 165 changed lines but only about 15 are real. Removing
the `if overlapType != Ignore:` level dedented the rest of the loop body; the contents
are character-identical apart from leading whitespace. `?w=1` shows the actual change.

### Verification

Output is unchanged. Comparing before and after on 1CRN, 9CSO (RNA with waters and a
ligand the monomer library does not know) and 1D3Z (10-model NMR ensemble), in both
kinemage and JSON output, about 25 MB total: every byte matches apart from the
`@caption` run-timestamp header.

clashscore2 agrees too. All ten per-model clashscores on 1D3Z match exactly and the
contact count is the same (41).

Timing on this machine:

| | before | after |
|---|---|---|
| probe dots, the three structures above | 56.8 s | 34.9 s |
| clashscore2 on 1D3Z | 27.0 s | 17.2 s |

`DotScorer::test()` gains a consistency case checking `check_dots()` against per-dot
`check_dot()` with the same filtering, comparing overlap type, overlap, gap, annular,
dot offset and cause identity. It passes, as do `tst_mmtbx_probe_ext.py` and
`mmtbx/regression/tst_probe.py`.

### Note

`CheckDotResult` grows by a `Point`, so its layout changes. Nothing outside
`Scoring.{h,cpp}` and the bindings references the type, but `mmtbx/reduce` links the
same `probelib` and will need the usual relink.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6421e8423fec`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.