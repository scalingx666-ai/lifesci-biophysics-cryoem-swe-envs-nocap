# Issue

**Title:** kinemage: probe dot failures, backbone links, footer and command-line fixes

## Description

Fixes for `molprobity.kinemage` / `phenix.kinemage`, one per commit:

- Probe dot failures were swallowed, so a kinemage could have no dots and no
  explanation (7jr5). Residues without restraints are now left out of the dots
  with an `@text` note (`omit_unrestrained_residues=False` stops instead), and
  `cif=` restraints reach reduce2 and probe2. All `cif=` files are loaded, and
  a missing model file raises Sorry.
- Backbone links used residue numbering, so insertion codes broke the chain.
  They now come from the conformation_dependent_library fragment generators.
- The footer switched off `{H-bonds}`, but probe2 names it `{H-bond}`. Adds a
  `vdw_dots` option.
- The output is named after the whole file name, and a second model file
  raises Sorry instead of replacing the first.
- Multi-model files get one animatable group per model, as
  `build_kinemage_from_model` already does.
- Registers `kinemage/tst_validation.py` in `mmtbx/run_tests.py`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b4d57bffc081`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.