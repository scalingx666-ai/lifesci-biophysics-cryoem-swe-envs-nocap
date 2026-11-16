# Issue

**Title:** iotbx.pdb: flip symmetric amino acids per conformer; flipped atoms keep their site values

## Description

# iotbx.pdb: IUPAC flip of symmetric amino acids per conformer, with all site values

## Problem
`flip_symmetric_amino_acids()` runs by default in `pdb_interpretation`, so in `model.process()`, and it puts atoms on the wrong names:
- An acid with one H: the O swap and the H stays, 2.2-2.3 A from its named O (6KK8 Glu A 167, 280). Same for Arg with blank NH and incomplete H in one conformer (7KCU A 69).
- Conformer B is renamed on conformer A's decision (1us0 A 120, A 276). A test spanning blank and altloc atoms is never made.
- A conformer made of several atom groups with one altloc is decided per group: Glu with its heavy atoms in one A group and HE2 in a second swaps OE1/OE2 without HE2 (OE2-HE2 1.000 -> 3.115 A), directly and through `model.process()`. The PDB reader merges such records, so this needs a hierarchy built by code.
- D ring of Phe and Tyr: they share one pair list, the D pairs are listed twice and swap back, D 3.2 A from the named C (6TAE, 7PRG).
- Only `xyz` and `b` follow the site. `uij`, occupancy, sigmas, `fp` and `fdp` stay with the name (1us0 Glu A 70).
- `phenix.pdbtools flip_symmetric_amino_acids=True`: same, because its scatterers were made before the flip. Combined with `rotate_about_axis` it also loses the rotation.

## Changes
Eight commits, on `2a7892cf3c`, head `da30e2ad65`. In order: `c6d54b3144` (1), `a955992541` (2), `e0f03a6df4` (3), `cf3f039d46` (4), `e14d68b51a` (5), `87f39e0179` (6), `1c7a9a047a` (7), `da30e2ad65` (8).
1. Per conformer: test and pairs per complete conformer (the blank atoms and all atom groups of one altloc, as in `residue_group.conformers()`). A pair swaps only if both atoms are in the conformer, a blank atom only if every conformer agrees; otherwise the residue is not flipped and named in the returned text with the reason. Adds the acid pairs (`HD1/HD2`, `HE1/HE2`, and D); `TYR` gets its own pair list. Flips are counted per residue.
2. Site values: a swap exchanges `xyz, sigxyz, occ, sigocc, b, sigb, uij, fp, fdp, resolution`, and `siguij` where stored; name, serial, element, charge, segid, hetero, i_seq and tmp stay. `pdbtools` applies the same swaps to its scatterers, so ADP and rotation edits made before the flip are kept.
3. `None` dihedral: coincident or collinear test atoms give no decision, with their own reason, instead of a `TypeError` (master has the same crash in a single group).
4. Idempotent decision: a conformer flips only if the partner scores better: smaller |chi| for dihedral tests, smaller volume delta for Val and Leu. A second call renames nothing. `tst_reduce2_report_renaming.py` pinned the old double swap and now expects none.
5. Near-identical dihedral conformers decided together; everything else stays per conformer. Only dihedral conformers whose two namings score within 30 deg of each other can join; this is an empirical naming tolerance (over the 333 files below, without a limit, the two namings differ by at most 10.5 deg for the near-identical Phe and by at least 85 deg for the others). Val and Leu never join: they keep their own chiral decisions. Two such conformers correspond if, for all common heavy-atom pairs, each atom is nearer one atom of the pair in the other conformer than its partner, by the same names or crossed; this joins them transitively (a contradiction gives no set) and does not depend on the current names. If their own decisions would not leave matching names, they take one of the two namings in which all match, by the occupancy-weighted score; a tie keeps the naming that renames fewer atoms.
6. `pdbtools`: the flip runs after `rotate_about_axis`, `adp`, `sites` (with `switch_rotamers`) and `occupancies`, so every selection sees the input names; xyz and occupancy go from the scatterers to the hierarchy before the decision, which uses nothing else. The help says so.
7. Reason: a protonated Asp/Glu says `protonated acid (one H on the O pair)`.
8. Test for several resnames on one position (2IZQ A 11).

Behaviour changes:
- Anisotropic models and models with different occupancies in a pair change output (ANISOU, occupancy, B on the renamed atoms and their riding H).
- Protonated acids (one H/D) and residues whose conformers disagree keep non-IUPAC names; they are named in the returned text.
- Tests spanning blank and altloc atoms are now made, so they add renames that master does not make: 3 residues in 2 of 333 files here (54-model set plus phenix_regression `pdb/`: `1OC2_tst.pdb` Glu A 49 and A 262, `2C30.pdb` Glu A 388); the review found 14 residues in 8 of ~2,300 files.
- Idempotent rule: a residue whose test atom is beyond 90 deg while its partner is further away no longer flips. Compared with commit 2: 45 conformer renames gone across those files, counting a model's deposited and stripped inputs separately (e.g. 1r27 Tyr C 77, 2b9v Asp C 271, 6KK8 Val B 260 with volume -0.38). Those conformers were renamed on every call: second-call renames 84 on master, 71 on commit 2, 0 now.
- Joint decisions occur at two residues in those files: 1us0 Phe A 276 flips together, also through placement and reduce2, and keeps CD1 A nearest CD1 B; pdb1jxt Phe A 13 stays as in the input (master did not touch it). Conformers crossed because they are different rotamers keep their own decisions, so they can stay crossed (1us0 Val A 67, 3cp5 Leu A 13, 2ERL Glu 21; 7KCU Arg A 10 A, B and C all flip).
- `pdbtools` with the flip: a selection now means the input name in every scope. Before, `occupancies` and `sites` saw the flipped names.
- The D ring of Phe/Tyr is now swapped.

Interface: new optional argument `swapped_pairs=None`; the return type stays `str`. The text changes: new reasons, a newline per residue, the insertion code in the residue name, and the MODEL id when there are several models (`Residue "MODEL 2 A GLU   11": not complete - not flipped`). New lines: `test atoms coincident or collinear`, `protonated acid (one H on the O pair)` (each `- not flipped`, with ` in altloc X` where it applies) and `altlocs A, B correspond, decided together - flipped`, `- not flipped` or `- flipped in altlocs B`. `not complete - not flipped` stays for other incomplete pairs.

## Companion: merge together
`phenix_regression` branch `iupac_flip_conformers` must be merged together with this PR. Its tests pin the old behaviour, so they fail with this PR alone.
| Commit | Content | Owner |
|---|---|---|
| `439c0da1d` | Wizard references: `test_autobuild_quick`, `test_map_to_object_and_sort_hetatms` and `_cif` | Tom |
| `7f1b0f5d9` | `tst_134`: a protonated acid stays in place | Pavel |
| `702066268` | `resolve_cryo_em` data: `model_A30_trans_100_shifted.pdb` and `.cif` | Tom |

Flip decisions on the inputs of `tst_134`, `test_autobuild_quick`, `test_map_to_object_and_sort_hetatms` (PDB, CIF) and `resolve_cryo_em` (PDB, CIF) are the same with commits 3-8 as with commit 2 (19 files), and no reference holds the changed reason text. Models made during the wizard runs are not covered by this check: **the server run below decides.**

## Tests
**Commits 3-8, local (macOS, head `da30e2ad65` on `2a7892cf3c`):** `tst_hierarchy_flip_symmetric.py`, `tst_hierarchy.py`, `tst_pdbtools.py`, `tst_reduce2_report_renaming.py` and 9 reduce2/hydrogen files that use the flip pass; also `iotbx/pdb/tst_hierarchy_flip_symmetric_2.py`, `mmtbx/regression/tst_flipping_his.py` and `tst_arginine_geometry.py`, which came with the base. New tests fail with the old code: `None` dihedral, second call, corresponding conformers on commit 2; `set_xyz` dropped; the flip back in `_process_sites`; `resnames[:1]`; `set_occ` dropped; a contradiction not excluded. Placement of 1us0 Phe A 276, the three-conformer test and the double-swap cancellation in `_NetRenames()` fail on the earlier name-based rule. Controls for the 30 deg limit (1us0 Val A 67, 3cp5 Leu A 13, a planted Val at volume -0.9, 2ERL Glu 21, 7KCU Arg A 10) fail without it; the planted Val also fails if Val/Leu may join within a volume limit. A boundary test (1us0 Phe A 276 with CA turned to chi2 60 in A, 120 in B; namings 59 and 58 deg apart) fails with a limit of 60 or 90. The in-process tests accept limits from 12 to 57; the scan's gap supports 30 within that range. Scan of 333 files, before the boundary test and the rebase (neither changes library code in the flip): no second-call rename, no crash; also the three-conformer test. `libtbx.find_clutter` on the 6 changed files: no report. **Not yet run:** the server with commits 3-8, the six companion tests, full suites.

Earlier runs, on commit 1-2:

Server, full suite, with commit 2 as `f673bde899` (before the atom-group fix), without the companion. Six failures occur only on the branch:
| Test | Cause | Companion fix |
|---|---|---|
| `refinement/tst_134` | By design, commit 1: it pinned the flip moving protonated Asp/Glu | Limits for the default run: mean 0.02, max 0.1 |
| `test_autobuild_quick` | By design, commit 2: `verybad.pdb` Asp 95 occupancy stays on its site (master moves 1.0 to the site deposited with 0.0). R/R-free of the first run 0.5438/0.5834 -> 0.5723/0.5360 | New reference, all three runs |
| `test_map_to_object_and_sort_hetatms`, `_cif` | By design, commit 2: Phe 93 CD1 0.84, CD2 0.99 stay on their sites | New references; each `ignored_output` gains the new `Total` timing line |
| `test_resolve_cryo_em`, `_cif` | Test data written by the old flip: in the shifted model, Phe 93 CD1/CD2 are exchanged with xyz only, each keeping the occupancy of its name | CD1/CD2 coordinates swapped back in both files, so no flip fires |

The explanations were checked locally (macOS), by overlaying master's flip, and on the server with the companion (see below).
- Wizard references: pass the real `check_results` in memory against the server's output. The autobuild reference was built from that same output.
- `tst_134`: passes with this branch, fails with master's cctbx at the first new assertion.
- `resolve_cryo_em`: with the corrected data (`69407414`, same files as `f31e8f87`), `test_resolve_cryo_em` and `_cif` pass on the server with this branch (`cb43ba8234`) and with master (`e8503d6486`), both CCoriginal 0.83546, CCnew 0.83545. Locally (macOS) both flips give 0.835436/0.838587; that local gap is the same with either flip and unrelated to this PR.

Local, macOS, on `410668abb1`, after the rebase:
- `iotbx/pdb/tst_hierarchy_flip_symmetric.py`, `iotbx/pdb/tst_hierarchy.py`, `mmtbx/regression/tst_pdbtools.py`, `mmtbx/regression/tst_reduce2_report_renaming.py`: pass.
- New: 14 functions in `tst_hierarchy_flip_symmetric.py`, 13 in process outside its `chem_data` gate; each fails with master's flip. 2 in `tst_pdbtools.py` failed on master at `692e9e59b6`; not rerun after the rebase.
- `libtbx.find_clutter` on the 5 changed files: no report.

Local, macOS, before the rebase:
- `tst_reduce2_report_renaming.py`: 5 functions updated for the acid rule. The double-swap cancellation now uses Asp/Glu without acid H and asserts that both swaps occur; it also passes on master.
- reduce2 `add` on 48 models, on `692e9e59b6`: 34 byte-identical to master, 14 differ, only as listed above and for 5 split residues (B no longer renamed on A's decision). Identical again after the atom-group fix, on `e8503d6486`.
- On `692e9e59b6`, not rerun since:
  - `iotbx` run_tests: 204 of 205 pass. `iotbx/pdb/hybrid_36_fem` does not start on this build (`Library not loaded: @rpath/libboost_thread.dylib`), before any of this code.
  - reduce2 suites (64 files) and `tst_add_hydrogen_time.py`: pass.
  - `phenix_regression/real_space_refine`: 70 of 75 pass; `tst_11, 34, 44, 48, 71` were stopped at a local 2 GB cap, not failures.

Server, with the companion (phenix_regression `69407414`) and cctbx `cb43ba8234`: `test_autobuild_quick`, `test_map_to_object_and_sort_hetatms`, `_cif`, `tst_134` pass; this run was independent of the one the references were built from. Not yet run: the server after the rebases onto `410668abb1` (which brings one C++ change, `57e4203d37`) and `2a7892cf3c` (no C++); planned before merging.

## Coding agent
| Field | Value |
|-------|-------|
| Model and version | Claude Opus 5.5 (claude-opus-5-5) |
| Harness and version | Claude Code 2.1.316, local CLI on macOS |

## Checklist
- [x] Searched open and closed PRs and issues for prior work (not done: `gh` is not installed here)
- [x] New or changed tests are registered in the module's `run_tests.py` (all three files are listed; new functions are called from their `__main__`)
- [x] Docstrings and PHIL `.help` strings cover the new or changed behavior
- [x] No new external dependency, or it was agreed with the developers
- [x] No imports of modules outside this repository; `chem_data` and `phenix_regression` are used only for data, with a skip when absent
- [x] Interface or breaking changes are called out under Changes
- [x] A human has reviewed the complete diff

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `058cc3e10f53`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.