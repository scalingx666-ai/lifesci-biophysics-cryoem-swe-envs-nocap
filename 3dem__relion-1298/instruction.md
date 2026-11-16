# Issue

**Title:** bugfix: 3d radial averaging

## Description

When doing 3D radial averaging, the average should be generated from from the entire 3d volume, not just repeatedly using the z = 0 plane of the image (indexed `0...img.zdim`). By only pulling from the z=0 plane while calculating the radius using the actual z coordinate of the voxel, this would result in an incorrect 3d radial average.

This [radial averaging function](https://github.com/3dem/relion/blob/master/src/jaz/image/radial_avg.h#L106-L153) (`RadialAvg::fftwHalf_3D_lin` in `src/jaz/image/radial_avg.h`) seems to only be used during particle reconstruction (both in STA and SPA) and also during 3D subtomogram extraction. It is called by the `Reconstruction::ctfCorrect3D_heuristic` function, to generate a radial average of the rings of the 3D CTF, and use the average to prevent any CTF values that are too low (0.001x lower than the average) respective to this average from being used for CTF correction (see: https://github.com/3dem/relion/blob/master/src/jaz/tomography/reconstruction.h#L527-L541)

Testing with real data during subtomogram particle reconstruction (this isn't very comprehensive), it seems like there is no significant difference between the output without the fix and with the fix, since this radial average is just used to prevent CTF values along that radius from being too low ([here](https://github.com/3dem/relion/blob/master/src/jaz/tomography/reconstruction.h#L527-L541) in the `Reconstruction::ctfCorrect3D_heuristic` function) but many (if not all) of the values along the radius are not less than 0.001x the average weight. In the case with synthetic data (generated using PolNet) run through a reconstruction without orientations (that holds no biological relevance) a discrepancy can be observed, but I'm not sure this is worth regarding. Nevertheless, for the sake of correctness, I thought this was worth mentioning. Thank you!

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `62bd8351e133`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.