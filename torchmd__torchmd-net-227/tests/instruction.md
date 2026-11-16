# Issue

**Title:** Unify torch extension compilation so that everything is compiled in setup.py

## Description

I wrote a unified mechanism to add TorchScript C++ extensions, which will be compiled in setup.py time and will be made available under the torchmdnet.extensions namespace.

I added the is_current_stream_capturing utility to it. This removes gcc and nvcc as runtime dependencies.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `50a0b6a0ab3f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.