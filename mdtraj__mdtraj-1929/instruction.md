# Issue

**Title:** Sort bonds to avoid make_molecules_whole failures

## Description

by adding a function which sorts the list of bonds such that they continuously traverse the molecular graph, thereby preventing wrapping atoms to disconnected neighbours in `make_molecules_whole`.

The function now uses a Breadth-First Search to order the bonds in near linear time complexity.

I did some quick tests, here is how it compares to the previous version I posted in the issue, and to python's `sorted` which it would replace, on a fully disconnected graph.

![image](https://github.com/user-attachments/assets/46275320-3aa5-4cfb-b6fa-ad45d651d746)

And here is a comparison between the `image_molecules` of this PR and master on a simulation of a slightly large system:

![image](https://github.com/user-attachments/assets/949d19a0-d460-41bb-af4b-6b7b3ab76ce5)

As a side-note, the BFS returns only the minimum number of bonds to connect all atoms in a molecule (minimum spanning trees), so technically there could even be performance improvements for densely connected molecular graphs as the loop iterates over less bonds.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b963d4f01290`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.