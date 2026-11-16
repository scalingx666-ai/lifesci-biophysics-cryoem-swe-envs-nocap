# Issue

**Title:** Refactor simulation.py for consistent Hoomd and OpenMM Simulation classes

## Description

### PR Summary: 
Refactor the simulation classes into objects that can be used to modify/update compound positions for initialization. Allows either to just use default values without a forcefield, or use GMSO/foyer forcefields, paired with forces_handler objects for easily rescaling/using dpd minimization forces. Finally, energy logging is possible, which helps users debug system configuration as they initialize.

### PR Checklist
------------
 - [x] Includes appropriate unit test(s)
 - [x] Appropriate docstring(s) are added/updated
 - [x] Code is (approximately) PEP8 compliant
 - [ ] Issue(s) raised/addressed?
 - [ ] Logger for energy and other simulation outputs
 - [ ] Integration with various forces_handlers
 - [ ] tests on hard to minimize structures

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `52a0897a946e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.