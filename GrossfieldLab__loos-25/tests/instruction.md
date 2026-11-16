# Issue

**Title:** Rgyr upgrade

## Description

This pull is about an updated version of the rgyr tool adding support for writing a time-series to file. It also now uses program options to implement IO, and therefore has an easier to use CLI and a help message that is more similar in function and appearance to other LOOS tools. It can now read trajectories in the MultiTraj mode, in particular. I also overloaded splitByMolecule of AtomicGroup fame so that it may now take an argument, which is a selection string. This should apply the selection to the groups after they are split, so that the connectivity for the AG from which the method was called will be used for the 'splitting' walk first, and then the selection is applied to the AGs before they are appended into the vector for return. It was useful for writing this particular tool, but it has been mentioned that this is necessarily the 'right' way to do things in general if true chemical connectivity (and not the connectivity of the AG) is desired. Because of this, I believe that this change would be a useful addition to the library.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `91e647c1f190`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.