# Issue

**Title:** Simplify XML entity escaping

## Description

Removes unsafe initialization and simplifying XML escaping. Replaces the five-entry XML escape map with a switch. This preserves output and avoids map lookups.

Benchmarked with MSVC x64 Release, alternating map/switch runs:

Escaping numeric/attribute inputs: 19% faster
Escaping mixed XML text: 23% faster
Memory usage: Essentially unchanged.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8a472e440cea`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.