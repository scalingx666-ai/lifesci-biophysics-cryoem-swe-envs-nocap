# Issue

**Title:** Allow model check to be skipped with `nequip-compile` via env var

## Description

This adds a `--skip-dtype-check` option to `nequip-compile` to skip the dtype similarity checks at the end of model compilation.

The motivation for this is that, when compiling on CPU at least, `test_model_output_similarity_by_dtype` spikes the memory demand. This prevents the use of `nequip-compile` on FasRC login nodes (which annoyingly have quite a low amount of RAM) due to OOM, which makes things a little bit awkward. Using this option, `nequip-compile` can run on the login nodes. Intended only really for power users / developers, when one is already confident there are no dtype issues with the models.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8224cf69967c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.