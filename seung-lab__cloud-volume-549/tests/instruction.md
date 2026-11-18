# Issue

**Title:** feat: add experimental zfpc codec to CloudVolume

## Description

This PR adds experimental support for the experimental [zfpc](https://github.com/seung-lab/zfpc) zfp container format. i.e. `"encoding": "zfpc"` in the info file.

This will allow for maximum compression of zfpc streams and defines new fields for Precomputed info file scales:

`zfpc_rate`, `zfpc_precision`, `zfpc_tolerance`, `zfpc_correlated_dims`

You must specify one or none of rate, precision, and tolerance (tolerance and rate are probably going to be the most preferred). `zfpc_correlated_dims` is a list of 4 booleans, where true means that axis index's data is correlated with other true dimensions.

For definitions of rate, tolerance, and precision, consult zfp documentation: https://zfp.readthedocs.io/en/latest/modes.html

For more information on zfpc and the correlated_dims parameter, consult the zfpc page: https://github.com/seung-lab/zfpc

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e48fae1fe768`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.