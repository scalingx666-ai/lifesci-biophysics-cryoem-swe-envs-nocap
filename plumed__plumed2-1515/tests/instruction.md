# Issue

**Title:** kenref: make the requested atoms whole before reading them

## Description

##### Description

`KENREF` needs whole molecules: it fits all `GUIDE_ATOMS` at once, and the pair distances have no cutoff. KEnRef (≥ 2.0.0) already does the setup work. It builds a spanning tree over the reference structure and defines `makeRequestedAtomsWhole()`, which moves each requested atom to the periodic image nearest its tree parent. Nothing called it, though. `getGuideAtomsX()` and `fillSubAtomsX()` read `getPositions()` directly, so on a molecule split across the box, KEnRef's split check stopped the run.

This PR makes three small changes to `src/kenref/KEnRefBias.cpp`:
- `getLocalModelX()` calls `makeRequestedAtomsWhole()` first;
- `getGuideAtomsX()` and `fillSubAtomsX()` read the repaired positions.

It also adds a paragraph to the `KENREF` documentation and a note in `module.md`. For builds of `master` made before this change, the note shows the `MOLINFO ... WHOLE` + `WHOLEMOLECULES` workaround.

Effect:
- Coordinates that are already whole are left unchanged, bit for bit.
- A reference that is itself split is refused. The tree then has an edge at least half the smallest box width, and KEnRef stops with an error naming the reference.

The declarations come from KEnRef's `KEnRefBias.h`, so this needs KEnRef ≥ 2.0.0. That is already the floor in `configure.ac` (`KENREF_MIN_VERSION`). v2.0.0 has both functions, and so do v2.0.1 and KEnRef `master`, the default for `KENREF_GIT_TAG`.

A second commit moves CI from KEnRef v2.0.0 to v2.0.1 (`.ci/install.kenref`), its newest release. The changes since v2.0.0 are all in KEnRef's GROMACS interface. The workflow resolves the tag to its commit for the cache key, so the cached v2.0.0 prefix is not reused.

New regtest `rt-kenref-sigma-torn`. It uses the GB3 fragment of `rt-kenref-sigma`, with residue 3 moved by −**b** and residue 5 by +**a**. The torn frame must give the same colvar and forces as the whole one, within 1e-6; the measured difference is ≤ 1e-11. The frames are `.xyz` rather than `.pdb`, because `--mf_pdb` stores single-precision coordinates. Without this change, the test fails (KEnRef refuses the split structure).

Checked locally against KEnRef v2.0.0 (1dccb2d):

| case | without this PR | with it |
|---|---|---|
| torn GB3 frame (KEnRef's `rt_validate_plumed.sh`), no `MOLINFO`/`WHOLEMOLECULES` | refused by the split check | E0 = 3.65155e-05, the GROMACS value; same as `MOLINFO WHOLE` + `WHOLEMOLECULES` (forces within 1e-10) |
| whole ubiquitin | 7.18869e-08 | 7.18869e-08, forces byte-identical to without |
| split reference | refused by the split check | refused by the reference check (longest edge 31.49 Å, half box 30.61 Å) |
| `--debug-dd` on 2 and 4 ranks, GB3 and ubiquitin | | forces byte-identical to serial |
| `--multi 2`, 2 ranks per replica vs 1 | | forces byte-identical |
| `regtest/kenref` (4 tests) | | pass |

Prepared with the help of Claude.

##### Target release

I would like my code to appear in release __2.11__ (`master`). `v2.10` does not have the kenref module.

##### Type of contribution

- [ ] changes to code or doc authored by PLUMED developers, or additions of code in the core or within the default modules
- [ ] changes to a module not authored by you
- [X] new module contribution or edit of a module authored by you

##### Copyright

- [X] the module I added or modified contains a `COPYRIGHT` file with the correct license information. Code should be released under an open source license. I also used the command `cd src && ./header.sh mymodulename` in order to make sure the headers of the module are correct.

##### Tests

- [X] I added a new regtest or modified an existing regtest to validate my changes.
- [ ] I verified that all regtests are passed successfully on [GitHub Actions](https://github.com/plumed/plumed2/actions).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c94b82d41108`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.