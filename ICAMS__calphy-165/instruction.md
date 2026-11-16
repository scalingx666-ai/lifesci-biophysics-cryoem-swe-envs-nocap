# Issue

**Title:** Add swap

## Description

This pull request introduces several enhancements and refinements to the `calphy` codebase, focusing on Monte Carlo swap functionality, composition scaling, and input handling. The changes improve functionality, add new features, and refine existing methods. Below is a categorized summary of the most important updates:

### Monte Carlo Swap Enhancements
* Added Monte Carlo swap functionality to `run_integration` in `calphy/alchemy.py`, including commands for defining swap groups, performing swap moves, and logging swap details. This feature supports both forward and reverse swaps between specified atom types. [[1]](diffhunk://#diff-9d6cd24ae6f91dc9efc78333ac1f31dd71f2bb9eb55a812069e0a3f3e7970cb0R239-R255) [[2]](diffhunk://#diff-9d6cd24ae6f91dc9efc78333ac1f31dd71f2bb9eb55a812069e0a3f3e7970cb0R311-R343)
* Introduced a new `MonteCarlo` class in `calphy/input.py` to define parameters for Monte Carlo swap moves, such as the number of swaps, swap types, and whether swaps are reversed. [[1]](diffhunk://#diff-e26fe9db0457458ad625dd6184d26a02aabfd9352c93feedb7c13e30d71847b0R77-R83) [[2]](diffhunk://#diff-e26fe9db0457458ad625dd6184d26a02aabfd9352c93feedb7c13e30d71847b0R136)

### Composition Scaling Improvements
* Added a new property `entropy_contribution` in `calphy/composition_transformation.py` to calculate the entropy contribution of a transformation.
* Implemented a `get_swap_types` method in `calphy/composition_transformation.py` to determine the atom types involved in swaps.
* Improved the `update_types` method in `calphy/composition_transformation.py` for more efficient handling of atom type updates.

### Input Handling and Validation
* Enhanced input validation in `calphy/input.py` to support composition scaling and phase diagram modes, including new attributes like `phase_name` and `reference_composition`.
* Added functionality to rename structure files based on unique identifiers during input validation.
* Integrated `tqdm` for progress tracking when reading multiple calculations from input files.

### Miscellaneous Updates
* Introduced a new CLI function `phase_diagram` in `calphy/clitools.py` for preparing inputs for phase diagram calculations.
* Added `kb` from `calphy.integrators` to `calphy/composition_transformation.py` for entropy calculations.

These changes collectively enhance the flexibility, usability, and performance of the `calphy` package.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f944cfcb842f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.