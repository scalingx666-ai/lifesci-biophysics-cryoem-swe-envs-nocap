# Issue

**Title:** LINCS constraint algorithm

## Description

I have a proposal implementation of the [Linear Constraint Solver algorithm (LINCS)](https://doi.org/10.1002/(SICI)1096-987X(199709)18:12%3C1463::AID-JCC4%3E3.0.CO;2-H) as an alternative to RATTLE/SHAKE. I mostly used the [Gromacs implementation](https://manual.gromacs.org/current/reference-manual/algorithms/constraint-algorithms.html#lincs) as a reference and tried to match the style and interface of Molly's [shake.jl](https://github.com/JuliaMolSim/Molly.jl/blob/master/src/constraints/shake.jl) implementation where it made sense. I'll be happy to receive comments/feedback and if wanted I can also add some text on how to use it to the docs.

Usage is pretty much the same as with SHAKE: You create a `LINCS` object which needs an array of masses, distance constraints and angle constraints (and optionally velocity and distance tolerances) and pass it to the system constructor as a tuple in the `constraints`-argument. It supports the two Gromacs arguments `nrec` for the expansion order of the coupling matrix inversion approximation and `niter` for the number of iterations for the corrections of the rotational lengthening. The implementation is GPU compatible (tested with CUDA).

My main motivation for this implementation was the support for ring topologies, which as far as I could tell are not supported with SHAKE.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2ae0859a3b2e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.