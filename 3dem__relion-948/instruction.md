# Issue

**Title:** repair relion_align_symmetry for C10,C11...C19 symmetry

## Description

Before:

```
$ relion_align_symmetry --i run_it300_class001.mrc --o aligned_C12.mrc --sym C12
 Reading map: run_it300_class001.mrc
 The selected map has been written to aligned_C12.mrc
 done!
```
i.e. not actually aligned as setting `--sym` to `C12` means `fn_sym` here: https://github.com/3dem/relion/blob/5088ae4a51b78df92c3fed994ea1b6851c1733a2/src/apps/align_symmetry.cpp#L195 contains the string `C1`

with fix:

```
$ relion_align_symmetry --i run_it300_class001.mrc --o aligned_C12.mrc --sym C12
 Reading map: run_it300_class001.mrc
 The input box size: 64
 Using the pixel size in the input image header: 3.736 A/px
 Center of mass: x= -2.66812 y= -0.0851064 z= -1.02361
 Re-centred to the centre of the mass
 Downsampled to the working box size 64 px. This corresponds to 3.736 A/px.
 Generating 400 projections taken randomly from a uniform angular distribution.
 Searching globally ...
  23/  23 sec ............................................................~~(,_,">
 The best solution is ROT = 107.387 TILT = 52.1369 PSI = 70.3549

 Refining locally ...
   7/   7 sec ............................................................~~(,_,">
 The refined solution is ROT = 107.387 TILT = 48.1369 PSI = 66.3549 DIFF2= 57.193

 Now rotating the original (full size) volume ...

 The aligned map has been written to aligned_C12.mrc
 done!
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5088ae4a51b7`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.