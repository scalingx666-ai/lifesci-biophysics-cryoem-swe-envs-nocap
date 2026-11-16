# Issue

**Title:** Improve MolProbity performance on very large models

## Description

ramalyze and reduce2 on very large models

Five small changes from validating very large models with MolProbity: integrative models with millions of atoms, such as PDB entry 9A1N with 1.9 million. Each commit is one change.

### ramalyze: linear in the number of residues

1. `mmtbx.validation.ramalyze`: the count and uniqueness keys are kept in sets instead of lists. They are only tested for membership, so the results are identical, but looking them up is no longer quadratic in the number of residues. ramalyze is shared by both MolProbity series.

| 9A1N subset | Residues | master | This branch |
|---|---:|---:|---:|
| 100k atoms | 12,040 | 3.5 s | 1.5 s |
| 200k atoms | 24,014 | 10.8 s | 2.9 s |
| 400k atoms | 47,884 | 43.4 s | 5.9 s |

Best of three runs each; the results are identical.

### reduce2: Optimizer linear in the number of Movers

2. `mmtbx.reduce.Optimizers`: the info string is collected in a list. `self._infoString += ...` on an attribute copies the whole string every time: on 9A1N subsets, 3.2 GB of copying for 25,920 atoms and 12.9 GB for 51,840, four times as much for twice the atoms. This branch copies none.
3. `mmtbx.reduce.Optimizers`: the interaction graph's edges are sorted into their cliques in one pass, instead of `_subsetGraph()` scanning the whole graph once per clique. On the same interaction graphs, building the cliques took 0.019 s for 25,920 atoms and 0.070 s for 51,840 on master, and takes 0.0002 and 0.0003 s now, with the same Movers and edges for every clique. The per-clique time limit from #1243 stays. `_subsetGraph()` is now unused; I left it in place in case anything outside uses it.

Neither changes the output: on both subsets, every hydrogen is bit-identical to master's. At these sizes the rest of reduce2 dominates its run time (about 50 and 95 s either way); the savings grow with the square of the model.

### reduce2: hydrogens independent of atoms far away

4. `mmtbx.reduce.Optimizers`: the spatial query is rebuilt after the Mover constructors move atoms (rotators turn their hydrogens to a starting orientation; tetrahedral methyls are staggered). `SpatialQuery.remove()` finds an atom by its current position, so the moved atoms stayed in the bins of their old positions, and neighbour searches depended on the bin layout, which depends on the bounding box of the whole model.
5. `mmtbx.reduce.Movers`: just-touch gaps within 1e-6 Å are ties, kept by the earlier angle. For a hydrogen that starts anti to its conventional friend, the orientations at ±120° have the same gap up to rounding, and the rounding depended on the extent of the model.

Two cases:

- A serine of 9A1N in a 12 Å sphere, run with and without 25 residues 500 Å away. On master, 2 of its 235 hydrogens differ between the two runs (ARG 182 H by 1.28 Å, LEU 274 H by 0.82 Å); on this branch, none do. ARG 182 H is a tie: on master its gaps at +120° and −120° are −0.020316251 and −0.020316266 Å, and the two runs give them the other way round.
- 9A1N subsets. In the 25,920-atom subset (chains L to L4), the hydroxyl of SER 280 in chain L3 has a CA-CB-OG-HG dihedral of −27.0°, on master and on this branch. Adding chains L5 to L9 (the 51,840-atom subset) moves it to −34.0° on master; this branch keeps −27.0°. In the larger subset, that hydroxyl and the one in chain L7 are the only hydrogens that differ from master's, and both come from commit 4; on the smaller subset, none differ.

### Testing

On current master (7c829f387), in a MolProbity build with chem_data's `chemical_components`, 45 tests give the same results before and after these changes. All pass except `mmtbx/regression/tst_reduce_switch.py`, which needs elbow (not in a MolProbity build) and fails the same way on master.

<details>
<summary>The 45 tests</summary>

- `mmtbx/reduce/Optimizers.py` and `mmtbx/reduce/Movers.py` (their self-tests), `mmtbx/reduce/tst_mmtbx_reduce_ext.py`
- `mmtbx/hydrogens/tst_add_hydrogen_{1..14,16}.py`
- `mmtbx/regression/tst_reduce.py`, `tst_reduce_timeout.py`, `tst_reduce_switch.py`, `tst_reduce_custom_bond.py`, `tst_reduce2_no_symmetry.py`
- `mmtbx/validation/regression/tst_ramalyze.py`, `tst_ramalyze2.py`, `tst_ramalyze_rare.py`, `tst_rama_z_01.py`, `tst_rama_z_02.py`, `tst_rotalyze.py`, `tst_utils.py`
- `mmtbx/validation/regression/tst_molprobity_{1..5}.py`, `tst_clashscore.py`, `tst_clashscore_2.py`, `tst_hydrogen_addition_clashscore.py`, `tst_do_flips_clashscore.py`
- `mmtbx/geometry_restraints/tst_ramachandran.py`, `mmtbx/kinemage/tst_validation.py`, `mmtbx/regression/tst_multi_residue_class.py`, `mmtbx/regression/tst_validate_ligands.py`, `mmtbx/conformation_dependent_library/tst_mutli_residue.py`, `mmtbx/regression/model/tst_model_atom_completeness.py`

</details>

These changes came out of [remolprobity](https://github.com/aozalevsky/remolprobity), which runs MolProbity's validators on a model in spatial blocks, so that memory is bounded by the block size rather than by the model. Its `stock` and `compare` subcommands are one way to check that a change leaves MolProbity's results as they were: `remolprobity stock` on 4HHB gives the same result with master's ramalyze and with this branch's. The self-check pair above is in remolprobity as `remolprobity/data/selfcheck_near.pdb` and `selfcheck_far.pdb`.

### Not included

- Series 1 clashscore has the same kind of quadratic list (`used`) and runs probe a second time for Coot's dots. clashscore2 already avoids both, so series 1 is left as it is.
- Memory: reduce2 peaks at about 22 KB and clashscore2 at about 54 KB per atom with hydrogens (on the 51,840- and 25,920-atom subsets), so whole-model runs of 9A1N would need about 80 and 200 GB. That takes more than small changes.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `950a774de190`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.