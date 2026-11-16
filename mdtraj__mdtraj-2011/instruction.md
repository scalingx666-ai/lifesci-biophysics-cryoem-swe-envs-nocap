# Issue

**Title:** Allow mdtraj.rmsd() to calculate without alignment

## Description

Users can now run RMSD without aligning. First pass around this, unless someone wants to work out the math described in 

Usage:
```
# Suppose you have `traj1` and `ref` md.Trajectory objects, which have different xyz coordinates

# RMSD without aligning
rmsd1 = md.rmsd(traj1, ref, superpose=False)  # RMSD without aligning

# RMSD after aligning manually
traj1.superpose(ref) # Align traj1 to ref, done inplace
rmsd2 = md.rmsd(traj1, ref, superpose=False)  # Calculate without alignment

# Let RMSD/Theobald QCP do the alignment
rmsd3 = md.rmsd(traj1, ref, superpose=True)  
rmsd3 = md.rmsd(traj1, ref)  # These two are equivalent

# Note `rmsd1` != `rmsd2` == `rmsd3`
```

<strike>I'm not sure how much slower this is, but I'll test it in a little bit.</strike> See EDIT 2 below

I also added tests for my additions, and updated `test_rmsd.py` so it no longer rely on the global instance pRNG. 

Annotated output showing the level of cythonization. (remove .txt)
[_rmsd.html.txt](https://github.com/user-attachments/files/19641225/_rmsd.html.txt)




====

EDIT:
I also tried doing a shortcut (similar to the [Theobald QCP code](https://github.com/mdtraj/mdtraj/blob/969b2e1f71f2dfb4f96230392f4925d9649ede11/mdtraj/rmsd/src/theobald_rmsd_generic.h#L74-L80)) where if the xyz are the same it would return 0. It compiles fine if I run `cythonize -a _rmsd.pyx` but it fails to compile via `pip install`. Not sure what's happening here, but any help would be great. I currently removed these lines so it works.

https://github.com/mdtraj/mdtraj/blob/969b2e1f71f2dfb4f96230392f4925d9649ede11/mdtraj/rmsd/_rmsd.pyx#L727-L729

Error:
```
      mdtraj/rmsd/_rmsd.cpp:22733:25: error: invalid operands to binary expression ('__Pyx_memviewslice' and '__Pyx_memviewslice')
       22733 | __pyx_t_1 = (__pyx_t_16 == __pyx_v_ref_xyz_frame);
             |              ~~~~~~~~~~ ^  ~~~~~~~~~~~~~~~~~~~~~
```

=====

EDIT2: Timing
Calculation is faster [9] than using QCP [8], but if you factor in superposing, It's about 4x slower [10]. Most of the run time is from superposing [11].

```
In [8]: timeit.timeit('mdtraj.rmsd(a,a,0)', setup='import mdtraj; a = mdtraj.load("traj.h5")', number=1000)
Out[8]: 0.025098250014707446

In [9]: timeit.timeit('mdtraj.rmsd(a,a,0, superpose=False)', setup='import mdtraj; a = mdtraj.load("traj.h5")', number=1000)
Out[9]: 0.012531082989880815

In [10]: timeit.timeit('a.superpose(a,0); mdtraj.rmsd(a,a,0, superpose=False)', setup='import mdtraj; a = mdtraj.load("traj.h5")', number=
       ⋮ 1000)
Out[10]: 0.12832299998262897

In [11]: timeit.timeit('a.superpose(a,0)', setup='import mdtraj; a = mdtraj.load("traj.h5")', number=1000)
Out[11]: 0.12596291699446738
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `22544580ef71`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.