# Issue

**Title:** fix(star): read simple data blocks instead of attaching the next loop to them

## Description

star.read() assumes every data block has a loop_. When a block holds
"_label value" pairs instead, it skips ahead to the next block's loop_,
returns that table under the simple block's name, and the next block's
own key is missing. If no loop_ follows, it raises AssertionError.

These blocks are common in RELION output: data_general in
postprocess.star and in RELION 5 tomography particles.star,
data_pipeline_general in default_pipeline.star, data_sampling_general in
run_it*_sampling.star, data_optimiser_general in run_it*_optimiser.star.

Repro with a RELION postprocess.star (data_general, data_fsc,
data_guinier):

    >>> from cryosparc import star
    >>> d = star.read("postprocess.star")
    >>> list(d)
    ['general', 'guinier']
    >>> d["general"].dtype.names
    ('rlnSpectralIndex', 'rlnResolution', 'rlnAngstromResolution', ...)

So d["general"] holds the FSC curve and d["fsc"] raises KeyError.

Fix: after a data_ line, skip blank and comment lines and look at the
first line. If it is loop_, read the block as before. Otherwise read the
"_label value" pairs as a one-row record array (using RLN_DTYPES for the
column types, the same as loop blocks), stop at the first blank line,
and put back a line that starts the next block. Stage 2 seeks past
simple blocks, so the following blocks are read under their own names.

Tested:
- New test_read_simple_blocks covers a simple block followed by loop
  blocks with and without a blank line in between. It fails before the
  change and passes after.
- pytest: 129 passed before, 130 passed after (with the LFS star file).
- Compared block names, row counts and values against starfile on 23
  files (starfile's test data, the RELION tutorial star files,
  tests/data/job096_run_data.star, and hand-made edge cases). Before:
  15 mismatches. After: 3, all expected: two files use quoted values
  with spaces, which star.read() did not support before either, and one
  is an empty data block that starfile itself mislabels.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `27a30a8f0e6d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.