# Issue

**Title:** DislocationAnalysis and SurfaceMesh: no more refused crystals; a tessellation that settles near ties alike in every cell

## Description

## The bug
`DislocationAnalysis` and `SurfaceMesh` refused valid input with "a vertex of a Voronoi cell is shared by other than three faces: replicate the system along its short periodic directions". This happened for:
- perfect fcc and cubic diamond crystals from 16³ cells (16 384 atoms) on;
- hcp, built dislocation models and `build_polycrystal` output, at random;
- thermal configurations, at random, more often the larger the system.

Both analyses build a Delaunay tessellation from the vertices of Voronoi cells. `Cell::cut` kept a point within the tolerance (1e-10 times the reach) of a cutting plane *in* the plane, so the vertex gained a fourth face. The 1e-4 Å jitter applied by both analyses makes such a coincidence rare per vertex, but not rare per system. The tolerance was also judged in each cell's own geometry, so the cells of neighboring atoms could in principle disagree.

## The fix
In the mode that collects vertices, a point within the tolerance window is now put on one side or the other, never in the plane:
- An in-sphere test of the five atoms decides the side. The atoms are taken in their sorted order and measured from the first, so every one of the five cells computes the same numbers and reaches the same decision.
- Where an edge from such a point crosses the plane, the new point is the circumcenter of its four atoms rather than an interpolation along an edge whose distance was overridden.
- Vertices on a wall give no tetrahedron, as before, whatever their number of faces.
- An image beyond int8 has its own error.
- Only the errors of a box that is too small keep the hint to replicate the system.
- `voronoi()` uses the original cut and is unchanged.

The design was worked out and prototyped with a second reviewer. In a sweep of 120 runs, the old code failed in 67 and the new code in none.

## Evidence
- New `tests/analysis/test_tessellation.py` checks the tessellation itself through a private binding:
  - every face is shared by two tetrahedra that agree on its atoms and images;
  - the volumes fill the box;
  - no atom lies inside the circumsphere of a neighbor;
  - no atom repeats in a tetrahedron.

  It covers perfect fcc, hcp, cubic diamond, lonsdaleite, bcc and sc; noise of 1e-5, 1e-3 and 0.05 Å with the tolerance widened 10 000 times, so the new code path runs thousands of times; a triclinic box; three kinds of open boundaries; a built polycrystal and a quadrupole of split dislocations; and perfect fcc of 16³ and 20³ cells analyzed by both analyses (these fail before the fix).
- The Voronoi volumes, the surface mesh (periodic and slab) and the dislocation analysis (lines, Burgers vectors, defect mesh) are bit-identical before and after on noisy inputs.
- The full suite passes (3576 tests). GCC passes with -Wall -Wextra -Wpedantic -Wshadow -Werror, and ThreadSanitizer passes the C++ checks.

Also: the docstring of `defect_mesh` said its normals point out of the crystal; they point into it, as the page says. The alpha comments now say what the number is (five Å times the cutoff, compared with the squared radius, as in OVITO).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2e239d4e8b28`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.