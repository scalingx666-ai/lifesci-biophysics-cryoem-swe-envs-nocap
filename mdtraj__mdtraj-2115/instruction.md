# Issue

**Title:** Fix to allow mdtraj.load() to load urls ending with pdb file extensions

## Description

Due to some overly-strict file-existence checks, you cannot open a url with a ".pdb" extension using generic `mdtraj.load()`. You have to go via `mdtraj.load_pdb()`.

This loosens it up. Though, due to how things work with the `mdtraj.load()` logic, I think we actually download the first file twice, if it happens to be a URL (an extra time for parsing topology).

## Before Fix
```
In [1]: import mdtraj

In [2]: a = mdtraj.load_pdb('https://files.rcsb.org/download/7X7S.pdb.gz')
/home/jml230/git/mdtraj/mdtraj/formats/pdb/pdbfile.py:214: UserWarning: Unlikely unit cell vectors detected in PDB file likely resulting from a dummy CRYST1 record. Discarding unit cell vectors.
  warnings.warn(

In [3]: a = mdtraj.load('https://files.rcsb.org/download/7X7S.pdb.gz')
---------------------------------------------------------------------------
OSError                                   Traceback (most recent call last)
Cell In[3], line 1
----> 1 a = mdtraj.load('https://files.rcsb.org/download/7X7S.pdb.gz')

File ~/git/mdtraj/mdtraj/core/trajectory.py:413, in load(filename_or_filenames, discard_overlapping_frames, **kwargs)
    410 # These topology formats do not support the 'top' keyword
    411 # This is to prevent the loader from reading the topology twice.
    412 if extension not in [".h5", ".hdf5", ".mol2"]:
--> 413     kwargs["top"] = _parse_topology(top, **topkwargs)
    415 # get the right loader
    416 try:
    417     # loader = _LoaderRegistry[extension][0]

File ~/git/mdtraj/mdtraj/core/trajectory.py:174, in _parse_topology(top, **kwargs)
    172 match _get_extension(top):
    173     case ".pdb" | ".pdb.gz" | ".pdbx" | ".pdbx.gz" | ".cif" | ".cif.gz" | ".mmcif" | ".mmcif.gz":
--> 174         _traj = load_frame(top, 0, **kwargs)
    175         topology = _traj.topology
    176     case ".h5" | ".lh5":

File ~/git/mdtraj/mdtraj/core/trajectory.py:316, in load_frame(filename, index, top, atom_indices, **kwargs)
    313     kwargs["top"] = top
    315 if loader.__name__ not in ["load_dtr"]:
--> 316     _assert_files_exist(filename)
    317 else:
    318     _assert_files_or_dirs_exist(filename)

File ~/git/mdtraj/mdtraj/core/trajectory.py:115, in _assert_files_exist(filenames)
    113 for fn in filenames:
    114     if not (os.path.exists(fn) and os.path.isfile(fn)):
--> 115         raise OSError("No such file: %s" % fn)

OSError: No such file: https://files.rcsb.org/download/7X7S.pdb.gz
```

## After Fix
```
In [1]: import mdtraj

In [2]: a = mdtraj.load_pdb('https://files.rcsb.org/download/7X7S.pdb.gz')
/home/jml230/git/mdtraj/mdtraj/formats/pdb/pdbfile.py:214: UserWarning: Unlikely unit cell vectors detected in PDB file likely resulting from a dummy CRYST1 record. Discarding unit cell vectors.
  warnings.warn(

In [3]: a = mdtraj.load('https://files.rcsb.org/download/7X7S.pdb.gz')
/home/jml230/git/mdtraj/mdtraj/formats/pdb/pdbfile.py:214: UserWarning: Unlikely unit cell vectors detected in PDB file likely resulting from a dummy CRYST1 record. Discarding unit cell vectors.
  warnings.warn(
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `986186a0e4bd`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.