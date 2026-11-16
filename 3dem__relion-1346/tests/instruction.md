# Issue

**Title:** Treat ".gain" gain references as TIFF on the generic image path

## Description

### Problem

A gain reference that is a TIFF but carries a non-TIFF extension — for example the `.gain` files written alongside some Falcon/EER datasets — fails in MotionCorr (own implementation) and in Bayesian Polishing when the movies are **non-EER (TIFF)**, with:

```
ERROR:
Invalid Spider file:  XXXX_GainReference.gain
in: src/rwSPIDER.h, line 151
```

The format is chosen from the file extension alone (`fImageHandler::openFile`), so an unrecognised extension falls through to the SPIDER reader in `Image::_read()`. The EER path already handles this case — `EERRenderer::loadEERGain()` appends `:tif` for `.gain` — and the GUI lists `.gain` as a valid gain extension (`*.{mrc,gain}`), so the file is expected to work; it just doesn't on the generic image path used for TIFF movies.

### Fix

When the extension is not a recognised image format, sniff the first four bytes and read the file as TIFF if it carries the TIFF magic (`II*\0` little-endian or `MM\0*` big-endian). Files with a known extension are untouched, and a non-TIFF unknown extension still falls back to SPIDER exactly as before — the change only ever *promotes* an otherwise-unhandled file to TIFF when the bytes confirm it.

This makes a TIFF gain read consistently on the generic image path, in the same orientation as the (readTIFF) movie frames, so the multiplicative gain correction stays pixel-aligned. The EER-only Y-flip reversal in `loadEERGain()` is intentionally not duplicated here: it exists to reconcile readTIFF orientation with the EER renderer's frame orientation, whereas TIFF movie frames already go through readTIFF, so no reversal is needed.

### Scope

- Restricted to read-only opens of existing files with an unrecognised extension.
- Fixes the shared `Image::read` path, so both MotionCorr and Bayesian Polishing pick it up.
- The EER path is unaffected (its `:tif` override means the extension is already recognised when `Image::openFile` runs).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `45f0a30fcc70`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.