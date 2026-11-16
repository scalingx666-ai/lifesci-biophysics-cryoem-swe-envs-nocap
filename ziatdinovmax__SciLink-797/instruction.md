# Issue

**Title:** Preview atomic structure files in the Files tab

## Description

## Summary

The Files tab showed `.xyz` and `.cif` as plain text and had no preview for `.extxyz`. Structure files — `.xyz`, `.extxyz`, `.cif`, `.vasp`, and `POSCAR` / `CONTCAR` — are now drawn on the server with ASE and shown with a **Rendered / Source** toggle. They go through the existing `/thumb` endpoint (`X-Preview-Kind: structure`), the same path the NPY/TIFF previews use, so no new JavaScript dependency is added.

## What changed

- **Same views as the agents.** The preview uses the viewing heuristics behind `view_structure`: surface plus two edge views for a slab, layer views for a layered crystal, plain x/y/z for a molecule with no cell. Those helpers move from `sim_agents/utils.py` into a numpy-only `utils/structure_views.py`, so the web server does not import the simulation agents (about 20 s); `sim_agents/utils.py` imports them back, so its behaviour is unchanged.
- **Depth cue.** Atoms farther from the viewer are drawn paler, so an atom hidden directly behind another shows through. Without it, a vacancy in one sulfur layer of MoS2 leaves no trace in a top view.
- **Slab orientation fix.** The vacuum axis is now the axis with the most empty space, the same comparison that decides a structure is a slab, rather than the longest cell vector. A 6×6 MoS2 sheet (a ≈ 19 Å, c ≈ 18 Å) had its "surface" view drawn edge-on. This also corrects the agents' own structure renders.
- **Edge cases.** A trajectory shows its last frame ("frame N of N" in the title). An unreadable file, more than 5000 atoms, or a missing ASE install returns a short reason instead of an image.
- **Chat links.** `.extxyz` and `.vasp` paths in a reply now open the file in the Files tab, as `.xyz` already did.
- **Docs.** `docs/react_web_ui.md` lists the new preview and the endpoint's new kind.

## Testing

- `tests/test_web_server.py::test_thumb_structure_files`: POSCAR, a three-frame `.extxyz`, a cell-less `.xyz`, and an unreadable file.
- `tests/test_structure_views.py`: a 6×6 MoS2 sheet is viewed down its vacuum axis.
- `test_web_server.py`, `test_structure_views.py` and `test_structure_build_tools.py`: 62 passed. `tsc -b` is clean.
- Checked by hand in the running web UI on generated structures (a MoS2 sheet with a sulfur vacancy plus O and W substitutions, a BaTiO3 cell, benzene, and a short trajectory). Each draws in 0.05–0.6 s.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5424e29c1f63`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.