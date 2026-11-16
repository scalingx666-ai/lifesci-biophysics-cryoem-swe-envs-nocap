# Issue

**Title:** Workaround for incorrect FFT results on Intel CPU OpenCL 2025.3

## Description

.

I didn't add a check to try to test for the buggy versions.  There is theoretically a way to distinguish between them by calling `clGetDeviceInfo` with `CL_DRIVER_VERSION`, but the result doesn't correspond directly with the installed version of Intel OpenCL:

- Release `2025.2.1-7` returns `2025.20.8.0.06_160000`
- Release `2025.3.0-639` returns `2025.20.10.0.10_160000`
- Release `2025.3.1-760` returns `2025.20.10.0.23_160000`

Even if we assume that version `2025.20.10.0.*_*` and up are bad, we'd have to parse this string, which would break if Intel ever changes the format of it in the future.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c24c619e4f1f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.