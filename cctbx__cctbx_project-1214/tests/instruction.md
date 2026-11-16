# Issue

**Title:** Fix altloc key in hierarchical JSON for multi-atom validation results

## Description

## Problem

`atoms.nest_dict` builds the `hierarchical_results` JSON by nesting on
`model_id`/`chain_id`/`resid`/`altloc`. The `atoms` class has none of those in
`__slots__`, so every level falls back to `getattr(self.atoms_info[0], level)`.

For the `altloc` level that is wrong whenever a result spans atoms with different
altlocs. The first atom may have a blank altloc while a later one belongs to conformer
A or B, so both conformers get filed under the blank altloc key and merged into a single
list, which breaks the one-result-per-altloc structure consumers expect.

Concrete example, the C-N-CA angle at residue 67 of 6m4j where residue 68 has A/B
conformers. Atom 0 is the C of residue 67 with a blank altloc, atom 2 is the CA of
residue 68 conformer A:

```
before:  altloc='' -> 11 entries   (both conformers merged in with 9 non-alt angles)
after:   altloc='' -> 9 entries
         altloc='A' -> 1 entry   score=-7.00  outlier=True
         altloc='B' -> 1 entry   score=+1.86  outlier=False
```

Only the A conformer is an outlier. Under the bug both were reported under one blank key,
so a consumer reading the hierarchy could not tell which conformer was bad.

## Scope

Every `atoms`-derived class that emits hierarchical JSON is affected:

| File | Classes |
|---|---|
| `mmtbx/validation/restraints.py` | `restraint` and its `bond`, `angle`, `dihedral`, `chirality`, `planarity` subclasses |
| `mmtbx/validation/clashscore.py` | `clash` (also used by `clashscore2.py`) |
| `mmtbx/validation/mp_validate_bonds.py` | `mp_bond`, `mp_angle` |
| `mmtbx/validation/rna_validate.py` | `rna_bond`, `rna_angle` |

The `residue`-derived validations (ramalyze, rotalyze, cbetadev, cablam, omegalyze,
suitealyze, undowser, undowser2) are **not** affected. They use `residue.nest_dict`,
where a real `altloc` slot exists and no fallback is taken.

`clash` is the worst case in practice: probe reports contacts between arbitrary atom
pairs, and `clashscore.py:313` sorts `atoms_info` by atom id string, so which atom
supplies the altloc key is effectively arbitrary.

## Fix

Special-case the `altloc` level to use the altloc shared by the result's atoms.

The new `get_consensus_altloc` deliberately does **not** assert when the atoms carry
conflicting altlocs, unlike the existing `get_altloc`. A probe clash can legitimately
pair a conformer A atom with a conformer B atom, and serialization must not raise on
that; it returns the blank key instead.

## Tests

Adds a regression test to `mmtbx/validation/regression/tst_mp_validate_bonds.py`. Its
existing `pdb_str_1` fixture already covers the case: ASP A 10 has A/B altlocs on
CG/OD1/OD2 while CB stays blank, so the CB-CG bond and the CB-CG-OD angles each mix a
blank atom with an alternate one.

The test asserts the general invariant that a result is filed under the altloc shared by
its atoms, plus the specific bucket counts for ASP 10. On unpatched code it fails with:

```
AssertionError: tst_mp_validate_bonds bonds result [' CB ', ' CG '] with altlocs
['', 'A'] is filed under altloc '', expected 'A'
```

It is added as a separate function called before `convert_pdb_to_cif_for_pdb_str`,
since that call renames the chain and would invalidate chain-specific assertions.

**No existing regression test changes value.** `count_dict_values` counts leaf
occurrences recursively through both dicts and lists, so relocating a result between
altloc buckets leaves every total unchanged. Verified by running `tst_mp_validate_bonds`,
`tst_mp_geo`, `tst_clashscore`, `tst_chiral_validation`, `tst_undowser`, `tst_undowser2`,
`tst_cbetadev` and `tst_restraints`, all passing. `libtbx.find_clutter` is clean on both
changed files.

(Unrelated, noted in passing: `tst_rna_validate.py` currently fails on
`len(rv.angles.results) == 47` returning 14. It fails identically without this change,
so it is pre-existing and not caused by this PR.)

## Note for reviewers

This changes the `hierarchical_results` output shape for the classes listed above:
results that previously appeared under `""` will now appear under `"A"` / `"B"`. Flagging
given the release timing. `flat_results` and `summary_results` are untouched.

There are some related JSON schema issues in this area that are **not** addressed here
(the `score` key having different signed/absolute definitions across modules, and the
`sigma` field name reading as a z-score when it is the restraint esd). I will raise those
separately on cctbxbb rather than bundle them into a bug fix.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `86dbe3c84028`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.