# Issue

**Title:** memory allocation size accuracy fix

## Description

without this fix i was seeing GPU runs crashing with errors like these:
- "illegal memory access" errors inside RELION's GPU allocator (custom_allocator.cuh)
- segfaults in CudaCustomAllocator::_free
- an out-of-memory report asking for 564 TB
- "No orientation was found as better than any other"
- NVIDIA Xid 31 and Xid 13 ("Out Of Range Address") in the kernel log, on two different GPU

with this fix it's working.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a85a7b92f44e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.