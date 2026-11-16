# Issue

**Title:** Fix ribbon surface normals, frame orthonormality, and spline resolution

## Description

## Summary

This PR fixes several long-standing issues in the MoleculesToTriangles ribbon rendering pipeline that produced geometrically incorrect output:

**1. Elliptical surface normals were wrong** (`CylindersPrimitive.cpp`)

The surface normal for an elliptical cross-section with semi-axes *a* and *b* should have components proportional to `sin(θ)/a` and `cos(θ)/b`. The code used `sin(θ)/√a` and `cos(θ)/√b` — neither the correct elliptical normal nor the simple radial direction. This produced incorrect specular highlights on all ribbon representations with asymmetric cross-sections (helices, strands).

**2. Spline-interpolated normal frame lost orthonormality** (`MolecularRepresentation.cpp`)

The normalOne/normalTwo vectors start as an orthonormal pair at each residue (from `evaluateNormals()`), but two subsequent operations destroy these properties:
- `smoothBetas()` independently polynomial-fits each x,y,z component, breaking both unit length and orthogonality
- Component-wise spline interpolation does not preserve vector magnitude or mutual orthogonality

The result: nominally circular tube cross-sections (coils) rendered as visibly elliptical, and ribbon orientations were subtly wrong throughout.

**Fix:** Apply full Gram-Schmidt orthonormalisation after spline interpolation at each sample point. The tangent is computed via central finite difference on the position spline, then projected out of both normal vectors to produce an orthonormal frame perpendicular to the tube axis. The spline-interpolated normals serve as smooth "hint" directions, preserving continuous variation along the curve while the Gram-Schmidt enforces geometric correctness.

**3. Arrow heads on short beta strands were distorted** (`MolecularRepresentation.cpp`)

The arrow taper occupies the second half of the last strand residue — precisely the region where the spline becomes unreliable on short strands (1–2 residues) with few control points. The spline path curves away sharply and the normal frame rotates, producing distorted arrow geometry.

**Fix:** At the arrow base, capture the position, tangent, and normal frame. Linearly extrapolate all subsequent arrow taper points from this frozen frame, giving clean straight arrow heads regardless of strand length. The extrapolated arrow tip position is carried forward so the next SSE element starts seamlessly where the arrow ends.

**4. Spline resolution was hardcoded at 6 samples per residue** (`NRStuff.h`, `DiscreteSegment.h/cpp`)

The pre-computed spline array used a hardcoded `accu = 6` regardless of the requested `ribbonStyleAxialSampling`. At higher sampling rates (e.g. 14 for publication-quality rendering), multiple query points mapped to the same spline entry, producing visible stepping/segmentation artefacts.

**Fix:** Plumb the axial sampling rate through `evaluateSplines()` → `calculateYDoublePrimes()` so the spline resolution matches what is requested. Existing callers (e.g. colour ramps) are unaffected via default parameter value.

## Files changed

| File | Change |
|------|--------|
| `CylindersPrimitive.cpp` | Fix elliptical normal: `1/√r` → `1/r` |
| `MolecularRepresentation.cpp` | Gram-Schmidt orthonormalisation, arrow extrapolation, pass sampling rate |
| `DiscreteSegment.h/cpp` | Accept `samplesPerSegment` parameter |
| `NRStuff.h` | Accept `samplesPerSegment` parameter (default 6) |

## Test plan

- [x] Load structure with mixed secondary structure (helices, strands, coils)
- [x] Verify coil tubes appear circular in orthographic projection (not elliptical)
- [x] Verify helix ribbons have correct specular highlights
- [x] Verify short strand (1–2 residue) arrow heads are clean and straight
- [x] Verify arrow tips connect seamlessly to adjacent coil representations
- [x] Verify rendering at elevated axial sampling (e.g. 14) produces smooth output
- [ ] Verify no regression at default sampling (6)

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8d5911379801`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.