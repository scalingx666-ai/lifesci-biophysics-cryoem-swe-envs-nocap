# Issue

**Title:** feat: n5 image reading plugin

## Description

This PR adds read support for the `n5://` format: https://github.com/saalfeldlab/n5/

The way neuroglancer implements N5 appears to be somewhat different from the [specification](https://github.com/google/neuroglancer/tree/c3c8af92cf539773e3ffdcb39d1268a0e0c54a6d/src/neuroglancer/datasource/n5) and had to be reverse engineered. 

The way it seems to be really working is there is a root `attributes.json` file that gives a listing of the resolution and the scales. Then each scale is listed as `s$MIP` and has its own `attributes.json` file which describes the data type, chunk size, compression type, and dimensions.

We support all compression types via cloudfiles except lz4. Only mode 0 datasets are currently supported.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `aa6794638129`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.