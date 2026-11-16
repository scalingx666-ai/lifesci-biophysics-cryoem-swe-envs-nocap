# Issue

**Title:** scitbx: honor numpy byte order when converting arrays to flex

## Description

The numpy bridge (`versa_flex_from_numpy_array`) dispatched on numpy's type number and copied raw bytes, ignoring the dtype's byte order. On big-endian s390x, every little-endian map file memory-mapped by mrcfile arrives as a `<f4` array, so `flex.float()` produced byte-swapped garbage that usually contains NaNs and triggers the `Input map file contains 'nan'` Sorry seen in the Debian CI log.

- Normalize every input with `PyArray_FROM_OF(..., NPY_ARRAY_CARRAY_RO | NPY_ARRAY_NOTSWAPPED | NPY_ARRAY_ENSUREARRAY)` before copying. numpy returns the input itself when it already qualifies, so native aligned arrays stay zero-copy; byte-swapped, unaligned, and subclass (e.g. `np.memmap`) arrays are converted first. On the foreign-order path this means a converted copy plus the copy into the result, which a comment in the bridge records. Non-contiguous arrays still raise as before.
- `tst_flex.py`: new `exercise_numpy_byte_order` checks `<` and `>` arrays for int, long, size_t, int16, uint16, uint32, float, double, complex_double, multi-dimensional shapes, NaN survival, and C-contiguous but unaligned input.
- `tst_mrc_io.py`: new `test_foreign_byte_order` writes the same map in native and opposite byte order and asserts `map_reader` and `DataManager` return identical data.

The byte-order tests fail before the fix and pass after it on any architecture, since they use a non-native array rather than requiring big-endian hardware. `tst_flex.py`, `tst_mrc_io.py` and `iotbx/ccp4_map/tst.py` pass locally (macOS, numpy 2.4.6).

MTZ reading is unaffected: libccp4 converts via the machine stamp, verified by reading hand-converted big-endian copies of the two regression MTZ files.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `063d29250a9a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.