# Issue

**Title:** DislocationResult.write: OVITO's crystal analysis files

## Description

`DislocationResult.write(path)` writes a dislocation analysis as a crystal analysis (CA) file of OVITO, version 6, which OVITO opens. The file holds:

- the box;
- the five structures with their families of Burgers vectors and the colors of `FAMILY_COLORS`;
- the crystal clusters with their structure, orientation and size;
- each line with its points, its Burgers vector in the lattice frame and its cluster;
- the junctions, as OVITO's rings of line ends;
- the defect mesh.

The suffixes `.gz`, `.bz2`, `.xz` and `.zst` compress the file, and it is written beside the target before taking its place, as `io.write` does.

Two conventions had to be matched, both checked against OVITO's own analysis of the same sample:

- **Triangle order.** OVITO runs the triangles of the defect mesh the other way round. On a crystal with a void, its mesh has signed volume −1142.58 Å³ and ours +1142.58 Å³, the same geometry. The file therefore lists corners 0, 2, 1, with the triangle beyond each edge, which OVITO needs.
- **Cluster orientation.** It maps the lattice frame to the box, spatial = O·b, as in MDAPY.

**Tests** (`tests/analysis/test_ca_files.py`): OVITO 3.17.1 opens each file, and the test compares what it reads with the result. Samples:

- crossing loops with a void (junctions and a defect mesh);
- dissociated fcc partials;
- bcc, cubic diamond, hcp and hexagonal diamond;
- partials ending on free surfaces (free ends);
- a closed bcc loop;
- a triclinic box;
- a perfect crystal.

What is compared:

- the cell, origin and periodicity;
- points, exactly;
- Burgers vectors in the lattice frame and in space (to 1e-5 Å, since OVITO keeps the orientations in float32);
- cluster of each line, lengths, and loop and infinite flags;
- each junction ring, through OVITO's connectors;
- clusters: number, structure and size;
- total length and the length of each family;
- defect mesh vertices and triangles.

A gzip-compressed file reads the same. Coverage of `_ca_files.py` is 100 %.

The DXA page gets a section "CA files", and its note on missing OVITO options no longer lists the CA export.

All 3852 tests pass locally.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `149f1c6a95e4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.