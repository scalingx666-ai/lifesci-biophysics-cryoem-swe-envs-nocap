# Issue

**Title:** `write("In Set Atom"){ set atom ...}` now uses the correct syntax.

## Description

This PR is a followup to  which enabled the `set atom` command to modify the LAMMPS DATA file directly.

This PR makes the following changes:
1) The `alchemical.py` script has been modified.  Now the script accepts this syntax: `set atom $atom:mols[2]/c1 ...`.  *(Previously, it used `set atom mols[2]/c1`.  If you were using this command without `$atom:`, you must update your `.lt` files.)*
2) These `set atom` commands must now appear in a section named `write("In Set Atom")`.  *(Previously, you would put them in `write("Postprocess")`.  *(If you were using `"Postprocess"`, you will have to update your `.lt` files.)*
3) I created an [example](https://github.com/jewettaij/moltemplate/tree/master/examples/all_atom/force_field_OPLSAA/set_atom_type_example), based on the (updated) example in the moltemplate manual.  (See below.)
4) I updated the documentation in the moltemplate_manual.tex file.
5) I changed the examples that contained wildcards (`"[*]"`), because I could not get wildcards to work.  If we are going to enable wildcards to be used with the `set` command, we should do it another way, because it affects the moltemplate.sh code as well.  I will leave this for a followup PR.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `faca4403fd84`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.