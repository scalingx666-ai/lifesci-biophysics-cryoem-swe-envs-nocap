# Issue

**Title:** fix visiblity determination for subtomo particle reconstruction

## Description

When doing subtomogram reconstruction, visibility of the extracted particles that are to be backprojected is inconsistently / incorrectly determined based on the box size after binning and does not account for the fact that the actual extracted box from the tiltseries is larger than the output box size when a bin factor is set.

The visibility should be based on the extracted dimensions before binning (so `bin factor * box size`, i.e. `binning * s`), to align with the later `TomoExtraction::extractAt3D_Fourier` call that extracts at the `s02D` dimension (just a couple lines after).

This also then aligns with the logic done in subtomogram extraction as well (using the pre-binned dimensions to determine particle visibility): https://github.com/3dem/relion/blob/master/src/jaz/tomography/programs/subtomo.cpp#L733  


~~Note: I'm still working through a bit of debugging as for why my re-implementation (done to support the reading of ZARR files & files on the cloud in a chunked manner) does not match numerically with RELION's output, so there might be more small bugfixes I suggest in the process of trying to align the two implementations (this code is will soon be open source). So feel free to hold off on a potential merge if you would rather not have me continue to open one-line PR changes, sorry about this. I thought I'd at least open this PR first, to bring attention to this small inconsistency. Thanks so much!~~

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `fe136118df1e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.