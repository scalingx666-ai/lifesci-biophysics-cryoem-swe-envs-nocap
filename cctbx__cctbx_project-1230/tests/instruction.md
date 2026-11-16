# Issue

**Title:** super_cell.py: complete and generalize get_siiu, speed up manager

## Description

# Complete the list of symmetry copies get_siiu finds

`get_siiu_sym_equiv` -> `master` · 3 commits · `cctbx/crystal/super_cell.py` (+255/-58), `cctbx/crystal/tst_sym_equiv_near_sites.py` (new, 666), `cctbx/run_tests.py` (+1)

---

`super_cell.manager` surrounds a model with its symmetry copies, so that a calculation on one region sees the crystal contacts around it. `get_siiu` decides which copies come close enough to be worth generating.

It was missing some. Each contact between two atoms was recorded from one side only, so a copy that reaches the model from the other side never got generated. On one of the test structures that costs a whole chain copy, with a contact at 6.7 A, well inside the 7 A search radius.

The search now goes through cctbx's pair tables, which already do this bookkeeping: both ends of every contact, atoms sitting on symmetry elements, and operators that are equal but written differently.

`get_siiu` can now also be asked about a chosen set of atoms instead of the whole model, which is what endo_exo and the water protonation work needs. Called the old way, it behaves as before.

Separately, `manager` looks up its mask for all atoms in one go instead of one call per atom. That was most of its runtime on large structures: 35 s to 2.6 s for 2000 atoms in I 41/a c d.

Nothing visible changes for users: on the five `tst_super_cell.py` structures the super sphere, which is what consumers read, comes out identical. Only the intermediate super cell grows, on the one structure with the missing operator.

The rest is detail, for the deeper read.

## Details

### `get_siiu`

It walked `neighbors_fast_pair_generator` directly, which lists each pair once from inside the ASU. It now builds a `pair_asu_table` and extracts a `pair_sym_table` with `all_interactions_from_inside_asu=True`, which lists pairs from both ends and keeps every operator of a group of symmetry-equivalent interactions. Operators that only appeared as the reverse of a pair were dropped before.

New arguments, all defaulting to the previous behaviour (whole hierarchy, `+1` buffer, 0.5 A tolerance):

- `sites_cart` and `selection`: the sites to search around, instead of always the whole hierarchy.
- `symmetry_tables`: prebuilt tables from the new `get_sym_equiv_tables`, so a caller querying several selections at one radius does not rebuild them each time. Neither table depends on the selection.
- `buffer`, `min_distance_sym_equiv`: the two constants that were hard-coded. `min_distance_sym_equiv_coincident` (1e-2) is there for callers that want a site written on a symmetry element at three decimals recognised, without 0.5 A snapping a site that is genuinely off the element.

What the result now guarantees:

- One operator per equivalent for an atom on a special position: the canonical member of the coset the site symmetry generates. A union of selections gives the union of the results, and which operator represents an equivalent does not depend on the selection.
- Operators carry `(1, 12)` denominators. `rt_mx` compares equal across differing denominators but does not hash equal, so the old `list(set(v))` dedup could keep an operator twice; the `all_ops` list deduped on `as_xyz` strings instead.
- An equivalent coincident with the site it came from is dropped, as before.

### Other changes

- `sym_equiv_sites_cart` replaces `apply_symop_sites_cart`: it takes the unit cell and works in Cartesian space, rather than taking fractionalization and orthogonalization matrices and round-tripping through a Python list.
- `manager` reads the super-cell mask for every atom at once. The per-atom `value_at_closest_grid_point` call was most of its time on a large structure: 11.0 s of 11.5 s for 2000 atoms in P 43 21 2, over the 176000 atoms of the super cell. On 2000-atom random structures: P 21 21 21 6.7 -> 0.6 s, P 43 21 2 10.0 -> 0.9 s, I 41/a c d 35.3 -> 2.6 s.

### Effect on `manager`

Atom counts on the `tst_super_cell.py` fixtures at radius 6, master vs this branch:

| fixture | operators | super cell | super sphere |
| --- | --- | --- | --- |
| huge_box | 0 -> 0 | 38 -> 38 | 38 -> 38 |
| one_chain | 7 -> 8 | 304 -> 342 | 160 -> 160 |
| 1yjp | 30 -> 30 | 2046 -> 2046 | 576 -> 576 |
| 3q2c | 7 -> 7 | 6296 -> 6296 | 1399 -> 1399 |
| fraction | 20 -> 20 | 1344 -> 1344 | 528 -> 528 |

`one_chain` picks up one operator that `get_siiu` was missing, a real contact at 6.662 A inside its own 7.0 A cutoff. It is masked out of the sphere, so `super_sphere_hierarchy`, which is what consumers read, is unchanged on all five.

### Testing

`cctbx/crystal/tst_sym_equiv_near_sites.py`, 24 exercises, checked against a brute-force enumeration over all space group operators and a block of lattice translations. There are two references: operators, for sites in general positions, and positions, for special positions where several operators give one equivalent. Fixtures cover a two-fold, a mirror in an F-centred group, a three-fold, an inversion centre and site symmetry m-3m, plus sites lying just off an element.

Run bare it sweeps the `debug_utils` space groups; given a symbol it runs that group alone. Registered in `cctbx/run_tests.py` as `["$D/crystal/tst_sym_equiv_near_sites.py", "I41/acd"]`, 1.8 s there. `cctbx/crystal/tst_super_cell.py`, as expanded in e8a16bf5, is unchanged by this branch and passes, 39 s.

### For reviewers

- The only consumers are `mmtbx/geometry_restraints/quantum_restraints_manager.py` and qrefine's `super_cell.py` wrapper, both through `manager`, and both read `super_sphere_hierarchy`.
- `mmtbx/regression/tst_flipping_his.py` was not run: it shells out to external programs.
- Preparatory consolidation for later PRs of endo_exo and water protonation heuristics.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e8a16bf59503`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.