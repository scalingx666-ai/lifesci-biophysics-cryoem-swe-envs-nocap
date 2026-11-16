# Issue

**Title:** Fix position-dependent curvature/torsion, split chains at breaks, and stop copying values at chain ends

## Description

## Summary

( )
While using Melodia in FleXgeo2, I found that curvature and torsion depend on a residue's position in the chain rather than only on its local geometry (details in  #4 ). This PR fixes that, along with two related problems in the same code path, and makes downstream functions handle the result:

1. **Ill-conditioned Chebyshev fit** (ae39b16). `calc_curvature_torsion` fitted the degree-10 Chebyshev series on the raw residue index, so the fit lost rank past ~10 residues without warning. Each window is now mapped to [-1, 1]. On an ideal α-helix, interior curvature went from 0.354-0.477 to a constant 0.5135-0.5139.
2. **Chain breaks** (b4d0723). One spline was fitted through each whole chain, bridging missing residues. Chains are now split where consecutive Cα atoms are more than 4.2 Å apart. Each segment gets its own spline, φ/ψ are `None` across a break, and one `ChainBreakWarning` per call lists the breaks. The four entry points that had their own copy of the per-chain loop now share `GeometryParser.calc_chain_geometry`.
3. **Chain ends** (da4dabd). The first and last residue copied their neighbour's curvature and torsion, and the first and last three residues shared one writhing value. A value is now computed only where its whole window fits inside the chain or segment, and is `NaN` elsewhere (first/last residue for curvature, torsion and arc length; first two/last two for writhing).
4. **`NaN` downstream** (7b6662d, 47b3549). `bfactor_from_geo`, `cluster_alignment` and `clustering.superimposer` treat `NaN` as "no value". In the clustering these residues get label `-1`, so they never form or join a conserved block.
5. **Packaging** (3c1211f). `import melodia_py` failed after `pip install melodia-py` because `nglview`, `ipywidgets`, `numba` and `joblib` were not declared. `numba` and `joblib` are now core dependencies, and the viewers import `nglview`/`ipywidgets` when called, through a new `viz` extra. The `fast` and `parallel` extras are kept so existing install commands still work.

The README has a "What's new (unreleased)" section (1451d7a) describing all of this for users.

## ⚠️ Results change

Curvature and torsion change for every residue past the first ~10 of each chain, and values near chain ends and breaks are now `NaN`. Results from v0.1.8 and earlier are not comparable with results from this version. Code that needs a complete table can use `df.dropna(subset=["curvature", "torsion"])`.

## Example data files and notebook outputs were not regenerated

To keep this PR reviewable, I did **not** re-run the notebooks or regenerate the files in `examples/`. The committed data files and the outputs stored in the notebooks were produced by the old code, so they show the old values, including copied end values and the position-dependent curvature. These include `model.dill`, `geo.dill`, `df.parquet.gzip`, `dfbio.parquet.gzip`, `out.pdb`, `7qcc_geo.pdb`, the `*_sup.pdb` and `*.csv` superimposition outputs, `cluster_models.pml`, `cluster_7qcc.pml`, `model.ps`, and the figures.

`model.dill` needs the most attention: notebooks 02 and 04 load it when it exists, so they show old geometry even with the new code. Deleting it makes the notebooks recompute it. When regenerated, one cell of notebook 02 (the one printing curvature and torsion with `:7.4f`) raises `TypeError`, on `main` as well: the parser stores `None` at alignment
gaps, while the committed `model.dill` predates that and stores `0.0`. I suggest regenerating these files in a follow-up commit or PR.

## Testing

- **New `tests/` suite (16 tests, pass on Python 3.13 and 3.10).**
  - The Chebyshev fix: values are constant along an ideal helix and unchanged by renumbering or by extending the chain at its start.
  - Chain breaks: each side of a break matches the same residues computed alone; φ/ψ are `None` across a break; a numbering jump alone is not a break; all entry points agree.
  - Chain ends.
  - `NaN` handling in `bfactor_from_geo` and `cluster_alignment`.

  12 of the 16 fail on `main`. The other four are guards that should pass there too:
  - an intact chain has no breaks;
  - a numbering jump alone is not a break;
  - two checks that `NaN` residues never form a conserved block in `cluster_alignment` (`main` has no `NaN`). These fail on the commit before 47b3549.
- **Refactor.** On 2LJ5 (no breaks), values outside the chain ends are bit-identical across the refactor. On data without `NaN`, cluster labels and B-factors are unchanged.
- **Packaging.** Checked in fresh virtual environments:
  - `pip install .` imports and computes geometry (including `n_jobs=2`) without `nglview`;
  - `[viz]` gives working viewers;
  - `[dev]` runs the tests.
- **Example notebooks.** I executed all eight on `main`, on the first commit of this PR, and on the final commit, and compared them cell by cell. Notebooks 02 and 04 were also run with `model.dill` removed.
  - No new errors, and every notebook produces the same number of figures.
  - Differences are the intended ones: values past residue ~10, and `NaN` at the ends.
  - In notebook 05, the number of clusters picked goes from 7 to 6, entirely because of the Chebyshev fix.
  - The `superimposer` RMSDs in notebook 06 are unchanged (5.32, 4.82, 4.57 and 4.60 Å).
  - The PyMOL script and PostScript alignment from notebook 02 are identical to those of the Chebyshev fix alone.
  - Notebook 06's coloured alignment varies between runs because the annealing is not seeded. With fixed seeds, the groups are identical.

## Not in this PR

- RNA chains are not split at breaks yet; this needs a validated C4'-C4' threshold.
- Natural-spline end effects: the residue next to each end is computed but biased (33% high on an ideal helix). This is documented in `calc_segment_geometry` and the README.
- I found some other problems while reviewing that are outside the scope of this PR, such as insertion codes, HETATM residues and gap labels in `cluster_alignment`. I'll open separate issues for them.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5ef09a2f9609`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.