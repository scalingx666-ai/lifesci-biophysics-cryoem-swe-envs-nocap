# Issue

**Title:** model: preserve stop_for_unknowns across manager.select()

## Description

## Summary

`mmtbx.model.manager.select()` rebuilds the selected model via the `manager(...)`
constructor, forwarding `crystal_symmetry`, `restraint_objects`, and
`monomer_parameters` — but **not** `stop_for_unknowns`. So a model created with
`stop_for_unknowns=False` silently reverts to the constructor default (`True`) after
any `.select()`. Downstream interpretation of the selected model then raises `Sorry`
on unknown residues that the caller intended to tolerate (e.g. the reduce2/clashscore2
pipeline with custom ligands).

This forwards `self._stop_for_unknowns` into the `select()` rebuild.

```python
       monomer_parameters         = self._monomer_parameters,
+      stop_for_unknowns          = self._stop_for_unknowns,
       expand_with_mtrix          = False,
```

### Scope

One line in `model.py` plus a regression in `tst_model_2.py`. The behavioral change is
narrow: models left at the default `stop_for_unknowns=True` are unaffected (the rebuilt
model was already `True`); only models that explicitly set `False` change — they now
correctly keep `False` across `select()`.

Split out from a larger custom-ligand fix so it can be reviewed on its own; the other
half (a prime/star atom-name synonym fallback in `pdb_interpretation`) is held back
separately as it overlaps ongoing nucleic-acid atom-name work.

## Testing

- New `exercise_select_preserves_stop_for_unknowns` in
  `mmtbx/regression/model/tst_model_2.py`: asserts `select()` preserves both the default
  `True` and an explicit `False`. It **fails on master** (selected model reverts to
  `True`) and passes with this change.
- `tst_model_2.py` full run passes, as do `tst_model_ncs`, `tst_model_mtrix`,
  `tst_model_biomt_mtrix`, and `tst_model_remove_alternative_conformations`.
- `libtbx.find_clutter` and `py_compile` clean.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `50ee96c76169`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.