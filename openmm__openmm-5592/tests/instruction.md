# Issue

**Title:** Size ComputeContext::clearBuffer thread count by int4 elements

## Description

This is the first small standalone PR split out of #5580.

`clearSingleBuffer` in `utilities.cc` zeroes memory in 16-byte `int4` elements (`words / 4`), but `ComputeContext::clearBuffer` and `getClearKernelSize` were launching `words` threads—four times as many threads as needed, with three-quarters of them immediately exiting the loop.

---

*Disclosure per `AI_POLICY.md`: AI coding assistants were used during kernel development, verification, and benchmarking of this change. The technical notes below were compiled with AI assistance and verified by the author.*

### What Changed

- **Match host launch size to `int4` kernel loop bound**: Updates `getClearKernelSize` and `ComputeContext::clearBuffer` in `platforms/common/src/ComputeContext.cpp` to size the launch by `(words + 3) / 4` `int4` elements instead of `words` 4-byte words (`+4 / -2` lines).
- **Standalone benchmark impact**: Because buffer clearing is a small fraction of total step time on its own, standalone Amber20 throughput is neutral (`0.999x` `single` geomean) while trimming unnecessary thread blocks before force evaluation.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4632f0f04975`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.