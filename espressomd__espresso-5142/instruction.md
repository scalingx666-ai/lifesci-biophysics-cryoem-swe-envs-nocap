# Issue

**Title:** Make short-range electrostatic force correction thread safe

## Description

Description of changes:
- Implement a thread-safe version of the short-range electrostatic force correction
  - In add_pair_force_corrections, the force is added to not `p.force()`, where `p` is a Particle pointer, but the thread-local variable `p1f_asym` and `p2f_asym`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `fefe8a0acdfd`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.