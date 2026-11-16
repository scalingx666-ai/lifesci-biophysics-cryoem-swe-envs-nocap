# Issue

**Title:** Allow mdtraj.load() to read various non-standard spec PDB files

## Description

Fixes  by allowing mdtraj to read pdb files with atom numbers > 99999 and residue numbers > 9999 written in vmd/hex convention (e.g., `186a0`), [chimera/hybrid36 convention](https://cci.lbl.gov/hybrid_36/) (e.g., `A0000`) and/or 'overflow' convention (e.g., `*****`)

This PR uses this  as a base, but I have significantly modified the code to account for:
1) reading non-decimal residue numbers. That PR only dealt with atom numbers. The present mdtraj code is already able to understand overflow in residue numbers (`****`) and those parts have been refactored into the `_overflow_residue_check()` function.
2) false negatives. The old PR only activates when `int(num_str)` fails out with a `ValueError`. However there are numbers that are in the overflow region that could be read as base-10 integers but are not what they appear to mean (false negatives like `2710` in base 16 means `10000` in base 10 but can be turned into the integer `2710`)
3) overflow in atom/residue numbers in asterisk. Added extra code to deal with overflow `*****` in atom numbers or `****` in residue numbers.

There are a lot of extra "checks" added in so I don't know if performance has taken a huge hit. Probably need another round of optimization but decided to start this PR so more people can help check.

I also didn't check if the old PR's "CONECT" sections work, but assuming they work because all of the CI passed.

Also  , where duplicate bonds could be made.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `43d8194bbc2e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.