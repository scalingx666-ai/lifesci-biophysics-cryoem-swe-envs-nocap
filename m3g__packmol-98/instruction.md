# Issue

**Title:** Initialize maxmove input variable for all file types

## Description

## Problem

I noticed failures to converge when using input files in the xyz format, with output steps like:
```
--------------------------------------------------------------------------------

  Starting GENCAN loop:            2
  Scaling radii by:    1.0000000000000000

  Packing:|0                                                        100%|
          |***

  Function value from last GENCAN loop: f = .12819E+01
  Best function value before: f = .12819E+01
  Improvement from best function value:    -0.00 %
  Improvement from last loop:    -0.00 %
  Maximum violation of target distance:     0.199288
  Maximum violation of the constraints: .29794E+00
  All-type function value: .12819E+01
--------------------------------------------------------------------------------

  Moving worst molecules ...
  Function value before moving molecules:   1.2818851334743486
  Type         1 molecules with non-zero contributions:    3.52%
  Moving         0 molecules of type         1
  New positions will be based on good molecules (movebadrandom is not set)
   Moving:|0                                                        100%|
          ||
  Function value after moving molecules:   1.2818851334743486

--------------------------------------------------------------------------------

```
and no change in function value between steps.

The number of molecules to move is set in heuristics.F90:
` nmove = min0(maxmove(itype),max0(int(nmols(itype)*frac),1))`

and I found that `maxmove` was all 0, as it was only being parsed for the PDB file format.

## This PR:
- Sets the default value of `maxmove` to be the number of molecules of the type (as the default is according to the docs)
- Parses `maxmove` from the `structure` section of input regardless of file type.

## Testing
- Tested that this fixed the systems I failed to build, both using the default and explicitly defining maxmove in the input.
- Tests in `testing/` pass.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a49235725cfe`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.