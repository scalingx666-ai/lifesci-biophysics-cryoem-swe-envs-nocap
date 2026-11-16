# Issue

**Title:** fix dump vtk parallel summary referencing wrong piece files

## Description

**Summary**

Fix the parallel summary file (`.pvtp`/`.pvtu`) written by `dump vtk` so it references
the correct per-processor piece files. Previously, for any dump file name containing more
than one `.` (e.g. `dump.reference.*%.vtp`), the `<Piece Source=...>` entries pointed at
nonexistent files (e.g. `dump_0.vtp`) and a spurious extra piece file was created, so the
distributed output could not be opened in ParaView without manual post-processing.

**Related Issue(s)**

No GitHub issue. Reported on the MatSci user forum:
https://matsci.org/t/66845 ("small problem with vtk files .pvtp").

**Author(s)**

Axel Kohlmeyer (akohlmey@gmail.com)

**Licensing**

By submitting this pull request, I agree, that my contribution will be included in LAMMPS
and redistributed under either the GNU General Public License version 2 (GPL v2) or the
GNU Lesser General Public License version 2.1 (LGPL v2.1).

**Artificial Intelligence (AI) Tools Usage**

This pull request was prepared with the assistance of an AI tool (Claude Code, model
Claude Opus 4.8). The AI performed the root-cause analysis and generated the new
`DumpVTK::write_pvtk()` and `DumpVTK::pvtk_piece_filename()` methods and the edits to
`write_vtp()`/`write_vtu()` in `src/VTK/dump_vtk.cpp` (and the matching header
declarations), under human review and direction. The XML structure was cross-checked
against the VTK XML file-format specification.

**Backward Compatibility**

No input-script changes. The names of the actual per-processor piece files are unchanged.
The change only corrects the *content* of the generated `.pvtp`/`.pvtu` summary file (which
was previously wrong) and stops creating one stray piece file per snapshot. Anyone who had
worked around the bug by post-editing the `.pvtp` no longer needs to.

**Implementation Notes**

Root cause: `write_vtp()`/`write_vtu()` delegated the summary file to VTK's
`vtkXMLPPolyDataWriter` / `vtkXMLPUnstructuredGridWriter`. Those classes derive the piece
file names from the summary file name by stripping everything after the *first* `.`
(`vtksys` "complete extension" behavior), so `dump.reference.000000.pvtp` yields the piece
prefix `dump` and references `dump_0.vtp`, `dump_1.vtp`, ... — which do not match the
per-rank files LAMMPS itself writes (`dump.reference.000000_0.vtp`, ...). The parallel
writer also emitted its own (wrongly named) piece data file as a side effect.

Fix: LAMMPS already writes all per-rank piece files itself with correct names, so the two
VTK parallel-writer code paths are replaced by a hand-written summary file:
- `DumpVTK::write_pvtk(int)` emits the `PPolyData`/`PUnstructuredGrid` XML directly
  (`VTKFile` header with runtime-detected byte order, `GhostLevel="0"`, `PPointData`
  declarations built from the existing `vtype`/`vector_set`/`name` maps using the same
  iteration as `reset_vtk_data_containers()`, `PPoints`, and one `<Piece>` per rank).
  `PPoints` precision is read from the live `vtkPoints` object so it matches the piece
  files rather than being hardcoded. No `PCellData` is written because all dump-vtk fields
  are point data.
- `DumpVTK::pvtk_piece_filename(int)` builds each `<Piece>` `Source` with the same
  `%`->`_<id>` substitution and `utils::star_subst()` timestep handling used to create the
  actual files, stripped to a path relative to the summary file.

The now-unused `vtkXMLPPolyDataWriter`/`vtkXMLPUnstructuredGridWriter` includes were
removed and `<cstdio>` added.

Verification: the file-name construction was checked with a standalone reproduction and
produces `dump.reference.000000_0.vtp` ... `_3.vtp` for the reported case, handles
sub-directory paths (relative reference) and the `dump*.%.suffix.vtp` pattern. NOTE: this
was not compiled/run against the VTK library locally (VTK is not installed in the
development environment used / `PKG_VTK=OFF`), so a build with `-D PKG_VTK=on` and a
multi-rank visual check in ParaView still needs to be confirmed by CI or a reviewer.

**Post Submission Checklist**

- [x] The feature or features in this pull request is complete
- [x] Licensing information is complete
- [x] Corresponding author information is complete
- [x] The source code follows the LAMMPS formatting guidelines
- [x] The feature has been verified to work with the CMake based build system

**Further Information, Files, and Links**

- Forum report: https://matsci.org/t/66845
- VTK XML file format reference: https://docs.vtk.org/en/latest/vtk_file_formats/vtkxml_file_format.html

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7f680de2968e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.