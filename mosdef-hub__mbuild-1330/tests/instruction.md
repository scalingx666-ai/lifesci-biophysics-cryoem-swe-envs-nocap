# Issue

**Title:** fix bug in scaling hoomd ffhandler

## Description

### PR Summary:
This PR should fix an issue where calling multiple simulation on a single hoomd ffhandler causes the forces to continually scale, as opposed to resetting back to their original value. 

Creating an ffhandler with `scale_bonds=0.1` would result in the correct value the first run, but then if you perform a second fire or capped_displacement run, it would be set to (0.1**2).

Likewise, if you continually created new ffhanders to run multiple steps of equilibration, things would still be incorrect. The original force would be still stored in the Hoomd Simulation object, which even with a new ffhandler, would have the value from the `scale_sim` method output from the previous ffhandler.

### PR Checklist
------------
 - [x] Includes appropriate unit test(s)
 - [x] Appropriate docstring(s) are added/updated
 - [x] Code is (approximately) PEP8 compliant
 - [ ] Issue(s) raised/addressed?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e7f71e1f97cc`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.