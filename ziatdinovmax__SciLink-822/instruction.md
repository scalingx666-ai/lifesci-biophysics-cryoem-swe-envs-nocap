# Issue

**Title:** Searchable index for structure collections in plan mode

## Description

.

## What it does

A folder of crystal-structure files, subfolders included, becomes a table plan mode can search:

```
folder of CIFs (any depth) ──index_structures──► CSV, one row per file ──► query_knowledge_data
```

Each row holds:
- the formula, declared or counted from the sites by occupancy, with its source;
- reduced formula, elements and metals;
- cell parameters, volume, volume per atom and density;
- the declared space group and crystal system, plus pymatgen's computed space group when pymatgen is installed, with a mismatch flag;
- temperature, mineral name and database ID;
- quality flags: partial occupancy, overlapping atoms (with the shortest distance between atoms that can coexist), carbon without hydrogen, and `sites_match_formula` (False when the sites differ from the declared formula, e.g. implicit hydrogens);
- `parse_error`. A file that cannot be read still gets a row and is listed in the tool result.

## Changes

- **`scilink/utils/structure_index.py` (new): the shared index.**
  - CIFs are read with ASE's tag parser (`parse_cif`) and expanded one site at a time.
    - Listed symmetry operations are applied directly.
    - Without operations, the group comes from the number or symbol. A rhombohedral group takes its axes from the cell; origin choice 2 is honoured.
    - A P1 cell skips expansion: a 4,000-atom P1 cell takes ~0.1 s.
  - Mixed sites keep every species.
  - Labels and type symbols follow pymatgen's `CifParser` rules (`Wat`/`Ow` → O, `CL1` → Cl, `O2-` → O), and Pauling File mixture symbols (`'0.648(4)Mn + 0.352(4)Fe'`) are split by fraction.
  - A CIF ASE cannot read is filled from pymatgen when it is installed; `parse_error` says so.
  - POSCAR/CONTCAR, xyz/extxyz and LAMMPS data go through `ase.io.read`.
  - `build_index` is incremental by mtime and size, rewrites the saved index only when something changed, and parses large sets in fresh interpreters (`run_in_child`).
- **Plan mode.**
  - New tool `index_structures(directory, recursive=True, refresh=False)` writes `knowledge/structure_index_<folder>_<hash>.csv`, which `query_knowledge_data` finds. Its result includes column notes for writing filters.
  - When `query_knowledge_data` or `screen_database` is pointed at a structure collection, it returns `index_structures_first` before asking for code-execution consent: a script that parses every CIF does not fit the query time limit. A folder where table records (CSV/JSON) rival the structures is not redirected.
  - Prompt entry 9c is added; existing entries are not reworded.
- **XRD local backend.**
  - `_cif_index` is a thin adapter over the shared index. Its API, parquet cache location and 200-file threshold are unchanged.
  - Its pre-filter uses pymatgen's own element set and computed space group, the same reading as the backend's full check, so it cannot drop a file that check would accept.
- **Ingestor: one rule for CIFs, however they arrive.**
  - 10 or more CIFs across the whole input (subfolders included) are a structure collection and are not embedded; one line says so and points to `index_structures`. Fewer are reference files and are read as text, both when named and when found in a folder (`ingest_files`, `extract_text`/`extract_document`).
  - Incremental paths decide the same way as a full rebuild: `kb add` counts the CIFs already in the KB's sources, and the planning agent counts the CIFs it ingested before.

## Where this differs from the issue's design, and why

- **Location.** The module is in `scilink/utils/`, not `scilink/knowledge/`. Importing `scilink.knowledge` takes ~14 s (the embedding stack), and every worker process would pay that; `scilink.utils` imports in under a second.
- **Every CIF is expanded site by site; non-P1 files do not go through `ase.io.read`.** `ase.io.read` keeps one species per mixed site, and its duplicate check scales with the square of the atom count.
- **The inspector redirects instead of only hinting.** A hint inside `_inspect_directory` would still let the generated script run into the timeout.
- **CIF collections are counted across subfolders, not per folder.** Collections are often one file per folder, which the per-folder database rule never catches, so a nested tree of thousands of CIFs would otherwise be embedded as text.
- **ASE as a core dependency** was already done on `main`.

## Testing

- **Real CIFs.** 98 files: pymatgen's `test-files` CIFs, ASE's test minerals, and COD entries.
  - **95/97 agree with pymatgen** on elements and occupancy-weighted atoms per cell. pymatgen cannot read the 98th.
  - The two differences are `'OH'`/`'OH2'` type symbols: pymatgen drops those sites, the index counts their oxygen.
  - No file is unreadable (3 are filled from pymatgen). About 15 ms per file with ASE only.
  - With pymatgen's element set, space group and lattice, the XRD pre-filter finds all 98 (0 misses).
- **New `tests/test_structure_index.py` (~50 tests), offline.**
  - Covers: symmetry expansion, mixed and split sites, overlaps, missing hydrogens, the P1 fast path, listed operations without a group name, rhombohedral axes from the cell, origin choice 2, fraction coordinates, ICSD `.` coordinates, label rules, mixtures, unreadable files, other formats, recursion, incremental rebuilds, workers, strict-JSON summaries.
  - Also covers the plan-mode tool and the redirect, including that no consent is asked, and the ingestor behaviour.
- **`tests/test_cif_index.py`:** all 16 pass on the shared index. One spy now targets the shared parser.
- **Related test files give the same results as on `main`:** structure backends, structure-matching tools, XRD fingerprint, ingest rules, planning prompt, tool-result clipping and plan payloads. `test_ingest_directory_rule::test_real_keyword_only_kb_end_to_end` and one test in `test_plan_payload_accessors` fail on `main` too.
- **Two independent review rounds; all findings fixed.**

## Notes

- **Not changed here: the XRD local backend's full check compares `str(el)` from pymatgen's composition.** For a CIF that declares oxidation states, that string is `Al3+`, not `Al`, so such files never match a chemistry query (verified on `main`: a corundum CIF with `Al3+`/`O2-` returns nothing for Al–O). This PR keeps that behaviour unchanged; filed as #823, with a small follow-up fix after this merges.
- **pymatgen space groups by default.** Plan mode computes pymatgen's space group when pymatgen is installed, as the issue specifies, except for cells over 2,000 atoms (seconds each); an explicit `compute_symmetry=True`, as the XRD backend passes, always computes it.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `bc5022878989`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.