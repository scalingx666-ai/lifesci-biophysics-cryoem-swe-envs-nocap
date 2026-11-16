# Issue

**Title:** use consistent tomogram centre in setProjectionMatrix

## Description

`TomogramSet::loadTomogram` sets `Tomogram::centre` with floating point division (`out.centre = d3Vector(out.w0/2.0, out.h0/2.0, out.d0/2.0)`), which `ParticleSet::getPosition` adds to convert `rlnCenteredCoordinate*Angst` into decentred tomogram pixels. `Tomogram::setProjectionMatrix` recomputes the same quantity with integer division, `specimen_center((double)int(w0/2), ...)`, for the `s0` translation. The two agree for even tomogram dimensions and differ by half a pixel for odd ones, leaving a residual of `centre_float - centre_int` of 0.5 px per odd axis in `projectionMatrices[f] * pos`.

This reaches subtomogram extraction through `TomoExtraction::extractAt2D_Fourier`, which projects the absolute 3D position to locate the crop centre in each tilt image. Odd tomogram dimensions are not prevented and can occur in practice. Supersedes #1286, which proposed the same change framed as a floating point accuracy improvement and was closed on the grounds that integer division is intentional for even-sized boxes. That applies to particle box sizes, which RELION constrains to be even; it does not apply to tomogram dimensions, which are unconstrained. .

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `260c6d2b5c58`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.