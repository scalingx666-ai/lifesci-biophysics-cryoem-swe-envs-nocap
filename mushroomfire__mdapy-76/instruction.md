# Issue

**Title:** read_dislocations: CA files read back as results

## Description

Builds on #75, whose commits it contains until that merges.

`mdapy.analysis.read_dislocations(path)` reads a crystal analysis (CA) file of OVITO, versions 5 to 7, compressed or not, as a `DislocationResult`. It reads the files `DislocationResult.write` writes and those OVITO writes. This makes a result kept on disk available again for statistics and rendering.

What the result holds:

- **Lines**: each with its points, Burgers vector in the lattice frame and cluster. Its family, length, loop and infinite flags are computed as the analysis computes them, its spatial Burgers vector from the orientation of its cluster, and its junctions from OVITO's rings of line ends.
- **Clusters** and the **defect mesh**, its triangles turned back to MDAPY's order with their periodic images.
- **Atoms**: none, since the file holds none. `structure` and `crystal_cluster` are empty, and `defect_atoms` is −1 at every vertex.
- **Crystal**: the one named in a `METADATA CRYSTAL` line, which the writer now adds and OVITO passes over. A file without that line, as OVITO writes, gives the structure with the most atoms in clusters, as OVITO decides it.

**Tests** (`tests/analysis/test_ca_files.py`):

- **Read back**: for 11 samples (junctions, a void, partials, bcc, both diamonds, hcp, free surfaces, free ends, a closed loop, a triclinic box, a perfect crystal), points and Burgers vectors are exact, as are cluster, family, length, flags and junction numbers. Junction points, clusters, lengths per family and the geometry and area of the mesh triangles also match.
- **OVITO's own files**: for the same samples, OVITO's dislocation analysis is exported as CA by OVITO 3.17.1 and read here. The comparison with OVITO's in-memory network covers the inferred crystal, points (OVITO writes 10 digits), Burgers vectors, clusters, loop and infinite flags, lengths, every junction ring, mesh triangles and the total length.
- **Malformed files**: a wrong header, version 4, missing numbers, an unknown keyword, an unknown cluster or crystal, a truncated file, rings that point past the lines or do not close, a file without clusters, a line of one point, and a triangle of a missing vertex. Each raises `FileFormatError` with its line number.
- **Other layouts**: further frames, the older `SIMULATION_CELL` layout, path and metadata lines, structures of other names, and a file without junctions.

Coverage of `_ca_files.py` and `_dislocations.py` is 100 %. All 3876 tests pass locally.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d15e3e346de4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.