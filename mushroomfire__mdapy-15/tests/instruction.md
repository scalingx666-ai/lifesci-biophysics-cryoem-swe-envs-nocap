# Issue

**Title:** Molecular polycrystals: keep molecules whole

## Description

, on the polycrystal of ice.

## What this changes

Until now `build_polycrystal` cut molecules at the grain boundaries, left H:O off 2:1 (2.024 in a trial of water), and dropped the `molecule` column and the topology.

**With a `molecule` column in the unit it now keeps molecules whole**, with no option to set:

- a molecule belongs to the grain that holds its center;
- atoms too close are removed by whole molecules, with the rule of `find_overlapping` applied to molecules;
- the atoms of one molecule never count as overlapping, and the default `min_distance` is 0.7 times the nearest distance **between** molecules, not the O-H bond;
- `molecule` numbers the molecules from 1, `image` tells how to put together those the box wraps, and bonds, angles and so on are kept, so a LAMMPS data file can be written at once;
- a molecule that crosses the box of the unit is put together first.

Also:

- The other columns of any unit, such as `charge`, now follow the atoms into the grains. `velocities`, `forces`, `id` and `image` do not.
- `find_overlapping` has an option `molecules`.
- A unit with a topology and no `molecule` column raises an error, and so does a term of a topology across molecules. The topology was silently dropped before.

New example: [A molecular polycrystal](docs/examples/molecular-polycrystal.md), cubic ice checked with `ChemicalSpecies` and CHILL+.

What it does not do: the hydrogen bonds at the boundaries do not follow the rules of ice, so a polycrystal needs relaxing with a potential for water. MDAPY has no builder for molecular crystals, so the unit comes from a file or from a few lines of code, as in the example.

## How the results were checked

- Polycrystal of ice, 100 Å box, 8 grains: 29805 molecules, 89415 atoms. `ChemicalSpecies`, which finds molecules from distances alone, gives H2O 29805 and nothing else. CHILL+ finds 20883 molecules of cubic ice, inside the grains.
- Tests: exactly 3 atoms in each molecule in the order O, H, H; O-H of 0.96 Å and the tetrahedral angle in every molecule, also across the box; each molecule in one grain; no atoms of different molecules closer than `min_distance`, with the default against its analytic value; 2 bonds and 1 angle per molecule, each within its molecule and of the right length; one grain gives the molecules of the whole box; a unit wrapped across its box gives whole molecules; columns of molecules.
- The LAMMPS data file was written and read back with all its bonds.

## Checklist

- [x] An issue describes this change: #11
- [x] `pytest` passes (3049 passed, 5 skipped locally); CI to confirm on Linux, macOS and Windows
- [x] `ruff check .` and `ruff format --check .` pass
- [x] Coverage is at least 95 % for each file changed (99.7 % and 100 %)
- [x] A new feature has its own documentation (example and section of the page) and its entry in the glossary
- [x] `mkdocs build --strict` passes

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `cc193fcab777`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.