# Issue

**Title:** Add a method to create Compounds from coxeter shape objects

## Description

This PR adds a new loader method to `conversion.py` that generates an mBuild compound from a Coxeter shape object. [Coxeter](https://coxeter.readthedocs.io/en/latest/) comes from the Glotzer lab, and is a great tool that generates vertices and edges of tons of different kinds of geometries ranging in complexity, which are converted to particles and bonds to create an mBuild Compound. Coxeter shapes provide relative coordinates, so the loader provides a scaling option to shift everything close enough to a target coordinate unit (which for mBuild is always nanometers). It also lets you set an element for the particles if desired, but this can be left blank so the particles are treated as generic beads.

Note: We might have to wait for a new release before we can use the coxeter conda-package as a dependency. I opened an  asking about a new release timeline.

I still need to add unit tests

### PR Checklist
------------
 - [x] Includes appropriate unit test(s)
 - [x] Appropriate docstring(s) are added/updated
 - [x] Code is (approximately) PEP8 compliant
 - [x] Issue(s) raised/addressed?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b49a9e440cc2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.