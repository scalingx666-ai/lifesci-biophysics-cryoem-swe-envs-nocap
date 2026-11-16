# Issue

**Title:** Fix load_many crash on single-atom/single-step FCHK optimizations

## Description

This PR fixes a  _LoadError_ in  `load_many` for single-atom or single-step FCHK optimizations where the trajectory block is missing.

**Changes :**
_iodata/formats/fchk.py_ : Added a fallback to construct a single frame trajectory from` Current cartesian coordinates`, `Total Energy`, and` Cartesian Gradient `when the standard trajectory block is absent but the file is an optimization (`FOpt`, `Scan`, `IRC`).

_Tests_: Added regression test  `iodata/test/test_fchk_single_atom.py` and data`  iodata/test/data/atom_opt.fchk
` to verify the fix and prevent regression.
<img width="474" height="208" alt="Screenshot 2026-01-23 213304" src="https://github.com/user-attachments/assets/8bd82c39-3945-4215-89e0-0b1e34f3e018" />

I ran a manual test script against the newly added regression data (`_iodata/test/data/atom_opt.fchk_ `) to verify the fix before submitting. As shown in the screenshot, `load_many `now correctly parses the file, extracting 1 `frame` with the expected energy **(-0.4665...)** and coordinates, instead of crashing.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `adab5813713b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.