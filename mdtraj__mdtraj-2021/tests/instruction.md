# Issue

**Title:** bugfix: delete_atom_by_index updates bond list

## Description

This addresses 

When `delete_atom_by_index` is called the bond list is now updated so that bonds involving the atom are removed.  

I've added a  test involving Ala3 . 

Tests for topology: 

```bash
tests/test_topology.py::test_topology_openmm SKIPPED (needs OpenMM)                                     [  3%]
tests/test_topology.py::test_topology_openmm_preserve_chain_id_when_specified SKIPPED (needs OpenMM)    [  6%]
tests/test_topology.py::test_topology_openmm_roundtrip_chain_id SKIPPED (needs OpenMM)                  [  9%]
tests/test_topology.py::test_topology_openmm_preserve_chain_id_even_when_empty SKIPPED (needs OpenMM)   [ 12%]
tests/test_topology.py::test_topology_openmm_boxes SKIPPED (needs OpenMM)                               [ 15%]
tests/test_topology.py::test_topology_openmm_formal_charges SKIPPED (needs OpenMM)                      [ 18%]
tests/test_topology.py::test_topology_dataframe_formal_charges PASSED                                   [ 21%]
tests/test_topology.py::test_topology_pandas PASSED                                                     [ 24%]
tests/test_topology.py::test_topology_pandas_TIP4PEW PASSED                                             [ 27%]
tests/test_topology.py::test_topology_pandas_2residues_same_resSeq PASSED                               [ 30%]
tests/test_topology.py::test_topology_numbers PASSED                                                    [ 33%]
tests/test_topology.py::test_topology_unique_elements_bpti PASSED                                       [ 36%]
tests/test_topology.py::test_chain PASSED                                                               [ 39%]
tests/test_topology.py::test_residue PASSED                                                             [ 42%]
tests/test_topology.py::test_segment_id PASSED                                                          [ 45%]
tests/test_topology.py::test_nonconsective_resSeq PASSED                                                [ 48%]
tests/test_topology.py::test_pickle PASSED                                                              [ 51%]
tests/test_topology.py::test_atoms_by_name PASSED                                                       [ 54%]
tests/test_topology.py::test_select_atom_indices PASSED                                                 [ 57%]
tests/test_topology.py::test_top_dataframe_openmm_roundtrip SKIPPED (needs OpenMM)                      [ 60%]
tests/test_topology.py::test_n_bonds PASSED                                                             [ 63%]
tests/test_topology.py::test_load_unknown_topology PASSED                                               [ 66%]
tests/test_topology.py::test_unique_pairs PASSED                                                        [ 69%]
tests/test_topology.py::test_select_pairs PASSED                                                        [ 72%]
tests/test_topology.py::test_to_fasta PASSED                                                            [ 75%]
tests/test_topology.py::test_subset PASSED                                                              [ 78%]
tests/test_topology.py::test_subset_re_index_residues PASSED                                            [ 81%]
tests/test_topology.py::test_molecules PASSED                                                           [ 84%]
tests/test_topology.py::test_copy_and_hash PASSED                                                       [ 87%]
tests/test_topology.py::test_topology_sliced_residue_indices PASSED                                     [ 90%]
tests/test_topology.py::test_topology_join PASSED                                                       [ 93%]
tests/test_topology.py::test_topology_join_keep_resSeq PASSED                                           [ 96%]
tests/test_topology.py::test_delete_atom_by_index PASSED                                                [100%]
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5b1e1e4475c1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.