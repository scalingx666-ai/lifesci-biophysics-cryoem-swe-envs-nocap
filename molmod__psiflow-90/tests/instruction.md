# Issue

**Title:** Replica exchange Hamiltonian swaps as bias

## Description

In i-Pi REX, force components under the `<forces>` tag are not swapped between replicas. Only force components under the `<bias>` tag can be exchanged (along with ensemble variables).

Here, we check the Hamiltonian of every replica, listing shared components (e.g., `MACEHamiltonian`) under  `<forces>`  and specific components (e.g., umbrella `PlumedHamiltonian`) under `<bias>`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2e07ab6791a0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.