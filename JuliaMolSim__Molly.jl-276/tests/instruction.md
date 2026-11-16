# Issue

**Title:** Use philox hash for DPD random force

## Description

This PR  by using the `randn_f32` from PhiloxRNG.jl instead of `Base.hash` which was not designed to have good statistical properties.

This PR also adds a `key` field to the `DPDInteraction` struct to change the random forces.

Using philox4x32_10 here is not ideal because it generates four random values when only one is needed. It is still reasonably fast, but there is probably room for improvement.

CC @sivasathyaseeelan

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ac668a98c73e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.