# Issue

**Title:** Reduce memory and runtime of Go contact map calculation

## Description

The RCSU Go contact map calculation (vermouth/rcsu/contact_map.py) could consume large amounts of RAM and take a long time on sizable protein complexes. Three changes address this without altering the resulting contact map:

- Compute the pairwise atom distance matrix once per molecule and share it between the OV and CSU contact searches, instead of running the same KDTree-based distance query twice at different cutoff radii.

- Rewrite _classify_contact_types to deduplicate and count (i, k) atom contact pairs per atom row with vectorized NumPy/SciPy operations instead of accumulating into Python dicts keyed by tuples, which carried large per-entry overhead for systems with many contacts.

- Build the Fibonacci sphere KDTree used in _calculate_csu_contacts once per molecule instead of once per atom by querying it in each atom's local (centered, unit-radius) frame rather than rebuilding an equivalent tree translated and scaled to that atom.

Validated against three real structures of increasing size, all producing identical Go contact counts to the unmodified code:
  - 6.4k atoms: wall time 17.2s → 10.9s (-36%)
  - 30k atoms: wall time 79.1s → 50.0s (-37%), peak Python memory 614.3MB → 521.5MB (-15%)
  - 36k atoms: wall time 65.8s → 41.2s (-37%)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `58b2dd1e7580`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.