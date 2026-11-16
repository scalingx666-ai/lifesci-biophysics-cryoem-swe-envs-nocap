# Issue

**Title:** Improved `pdb_selaltloc`

## Description

This one was tough.

**Done:**
* the previous `pdb_selaltloc` API is maintained
* the previous positive behaviour is maintained (all previous tests pass without change)
* issues reported in #115 are solved. You can run the example and see.
* Added additional tests for `dummy.pdb`. Now `altloc` inside residues, that is `altloc` atoms within residues also work.
* Complete rewrote the internals. Before there were two functions to select `altloc` by identifier or by occupancy. Now there is a single function to collect data from the PDB and two different functions to flush by id or by `occ`. There are two additional functions in case the altloc group to flush belongs to a single residue (technicalities).
* This was possible because determining when we move to the next `altloc` group is done by a boolean expression, hence it is more general and can be used even outside `pdb_selaltloc`.

The script might look more complex at first, but if you flow it is not, simply there are so many cases. We start at one function and we branch down to the `yield` statements. Some yield statements are tricky because of the `ANISOU` extra lines.

@JoaoRodrigues I really can't think about additional cases. You had already many different cases in `dummy_altloc.pdb` and `dummy_altloc2.pdb` and all those are covered.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2fecdebd7520`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.