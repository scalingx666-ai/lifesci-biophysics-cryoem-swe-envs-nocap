# Issue

**Title:** limit warnings in tests

## Description

This removes the following warnings from tests. Part of a de-cluttering process.


These are hidden:
```
/Users/runner/micromamba/envs/mdtraj-test/lib/python3.13/site-packages/mdtraj/core/topology.py:97: UserWarning: atom_indices are not monotonically increasing
    warnings.warn("atom_indices are not monotonically increasing")
```

```
tests/test_utils.py::test_ensure_type_11
  /home/runner/micromamba/envs/mdtraj-test/lib/python3.13/site-packages/mdtraj/utils/validation.py:123: TypeCastPerformanceWarning: Casting  dtype=int64 to <class 'numpy.float32'> 
    warnings.warn(

```

```
tests/test_dssp.py::test_1[1vii.pdb]
tests/test_gro.py::test_read_write
tests/test_gro.py::test_read_write_precision
tests/test_gro.py::test_load
tests/test_shape.py::test_principal_moments
tests/test_shape.py::test_asphericity
tests/test_shape.py::test_shape_metrics
tests/test_pdb.py::test_1vii_url_and_gz
  /home/runner/micromamba/envs/mdtraj-test/lib/python3.13/site-packages/mdtraj/formats/pdb/pdbfile.py:208: UserWarning: Unlikely unit cell vectors detected in PDB file likely resulting from a dummy CRYST1 record. Discarding unit cell vectors.
    warnings.warn(
```
-----
This test is modified so it doesn't throw this anymore.
```
 tests/test_geometry.py::test_dihedral_indices
tests/test_geometry.py::test_dihedral_indices
  /Users/runner/micromamba/envs/mdtraj-test/lib/python3.13/site-packages/mdtraj/geometry/dihedral.py:237: DeprecationWarning: Passing a Trajectory object to _atom_sequence isdeprecated. Please pass a Topology object
    warnings.warn(
```

----
Theses warnings are captured and the test will fail if the warning(s) was/were not raised. It's part of what the test is testing :)
```
tests/test_pdb.py::test_blank_indexing
  /home/runner/micromamba/envs/mdtraj-test/lib/python3.13/site-packages/mdtraj/formats/pdb/pdbstructure.py:204: UserWarning: Need to guess atom number       starting from atom 100000.
    warnings.warn(

tests/test_pdb.py::test_blank_indexing
  /home/runner/micromamba/envs/mdtraj-test/lib/python3.13/site-packages/mdtraj/formats/pdb/pdbstructure.py:273: UserWarning: Need to guess residue number string      starting from residue10000, atom 100000.
    warnings.warn(
```


```
tests/test_contact.py::test_contact_0
tests/test_contact.py::test_contact_4
  /home/runner/micromamba/envs/mdtraj-test/lib/python3.13/site-packages/mdtraj/geometry/contact.py:220: UserWarning: selected topology includes at least one glycine residue, which has no heavy atoms in its sidechain. The distances involving glycine residues will be computed using the sidechain hydrogen instead.
    warnings.warn(

```

```
 tests/test_gro.py::test_load
tests/test_topology.py::test_topology_pandas_2residues_same_resSeq
  /home/runner/micromamba/envs/mdtraj-test/lib/python3.13/site-packages/mdtraj/formats/gro.py:364: UserWarning: WARNING: two consecutive residues with same number (GST, HST)
    warnings.warn(

```


----
These tests are expected to generate NaN/divide by zero errors and we're hiding them.

```
tests/test_contact.py::test_contact_3
  /home/runner/micromamba/envs/mdtraj-test/lib/python3.13/site-packages/mdtraj/geometry/contact.py:271: RuntimeWarning: overflow encountered in exp
    np.exp(

tests/test_contact.py::test_contact_3
  /home/runner/work/mdtraj/mdtraj/tests/test_contact.py:117: RuntimeWarning: divide by zero encountered in scalar divide
    beta / np.log(np.sum(np.exp(beta / maps[t, r0, r1]))),
```

```
tests/test_drid.py::test_drid_2
  /home/runner/work/mdtraj/mdtraj/tests/test_drid.py:62: RuntimeWarning: divide by zero encountered in divide
    recip = 1 / squareform(pdist(t.xyz[i]))

```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c1eb7f6970d1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.