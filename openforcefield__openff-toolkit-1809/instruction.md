# Issue

**Title:** Track test durations and update "slow" tags

## Description

- [x] Track duration of slowest tests
- [x] Update "slow" tags to reflect slow tests


This only trims about 40 seconds off of the longest job, but provides some safeguards for accidentally introducing long tests.

Here are timings without this change:

Local results (using different arguments):

```
==================================================== slowest 20 durations ====================================================
128.18s call     openff/toolkit/_tests/test_toolkits.py::TestRDKitToolkitWrapper::test_max_substructure_matches_can_handle_large_molecule
47.54s call     openff/toolkit/_tests/test_examples.py::test_examples[/Users/mattthompson/software/openff-toolkit/examples/conformer_energies/conformer_energies.py]
32.81s call     openff/toolkit/_tests/test_topology.py::TestTopologyVisaulization::test_visualize_basic
26.88s call     openff/toolkit/_tests/test_forcefield.py::TestSmirnoffVersionConverter::test_read_smirnoff_spec_freesolv[0_1-2501588-0_0_2-True]
25.25s call     openff/toolkit/_tests/test_forcefield.py::TestForceFieldParameterAssignment::test_parameterize_protein[toolkit_registry0]
24.30s call     openff/toolkit/_tests/test_molecule.py::TestMoleculeResiduePerception::test_perceive_residues_natoms_t4[True]
21.54s call     openff/toolkit/_tests/test_molecule.py::TestMolecule::test_assign_partial_charges[ambertools-am1-mulliken]
20.81s call     openff/toolkit/_tests/test_molecule.py::TestMolecule::test_assign_partial_charges[ambertools-am1bcc]
20.66s call     openff/toolkit/_tests/test_forcefield.py::TestForceFieldParameterAssignment::test_parameterize_protein[toolkit_registry1]
19.18s call     openff/toolkit/_tests/test_forcefield.py::TestSmirnoffVersionConverter::test_read_smirnoff_spec_freesolv[0_1-1770205-0_0_2-False]
18.95s call     openff/toolkit/_tests/test_forcefield.py::TestSmirnoffVersionConverter::test_read_smirnoff_spec_freesolv[0_1-4936555-0_0_2-False]
18.86s call     openff/toolkit/_tests/test_forcefield.py::TestForceField::test_parameterize_large_system[propane_methane_butanol_0.2_0.3_0.5.pdb-toolkit_registry1]
17.53s call     openff/toolkit/_tests/test_molecule.py::TestMoleculeResiduePerception::test_perceive_residues_natoms_t4[False]
16.81s call     openff/toolkit/_tests/test_forcefield.py::TestSmirnoffVersionConverter::test_read_smirnoff_spec_freesolv[0_1-7754849-0_0_2-False]
16.46s call     openff/toolkit/_tests/test_forcefield.py::TestSmirnoffVersionConverter::test_read_smirnoff_spec_freesolv[0_1-7829570-0_0_2-True]
15.56s call     openff/toolkit/_tests/test_forcefield.py::TestSmirnoffVersionConverter::test_read_smirnoff_spec_freesolv[0_1-2518989-0_0_2-False]
15.40s call     openff/toolkit/_tests/test_molecule.py::TestMolecule::test_qcschema_molecule_record_round_trip_from_to_from
14.90s call     openff/toolkit/_tests/test_forcefield.py::TestSmirnoffVersionConverter::test_read_smirnoff_spec_freesolv[0_1-5282042-0_0_2-False]
14.62s call     openff/toolkit/_tests/test_forcefield.py::TestSmirnoffVersionConverter::test_read_smirnoff_spec_freesolv[0_1-6522117-0_0_2-False]
14.62s call     openff/toolkit/_tests/test_forcefield.py::TestForceField::test_parameterize_large_system[propane_methane_butanol_0.2_0.3_0.5.pdb-toolkit_registry0]
```

CI results:
```
============================= slowest 20 durations =============================
111.15s call     openff/toolkit/_tests/test_toolkits.py::TestRDKitToolkitWrapper::test_max_substructure_matches_can_handle_large_molecule
42.78s call     openff/toolkit/_tests/test_molecule.py::TestMoleculeResiduePerception::test_perceive_residues_natoms_t4[True]
42.42s call     openff/toolkit/_tests/test_molecule.py::TestMoleculeResiduePerception::test_perceive_residues_natoms_t4[False]
34.34s call     openff/toolkit/_tests/test_topology.py::TestTopologyVisaulization::test_visualize_basic
19.71s call     openff/toolkit/_tests/test_molecule.py::TestMolecule::test_assign_partial_charges[ambertools-am1bcc]
18.40s call     openff/toolkit/_tests/test_forcefield.py::TestSmirnoffVersionConverter::test_read_smirnoff_spec_freesolv[0_1-2501588-0_0_2-True]
17.22s call     openff/toolkit/_tests/test_molecule.py::TestMolecule::test_assign_partial_charges[ambertools-am1-mulliken]
16.83s call     openff/toolkit/_tests/test_forcefield.py::TestForceFieldParameterAssignment::test_parameterize_protein[toolkit_registry1]
16.69s call     openff/toolkit/_tests/test_forcefield.py::TestForceFieldParameterAssignment::test_parameterize_protein[toolkit_registry0]
16.49s call     openff/toolkit/_tests/test_forcefield.py::TestSmirnoffVersionConverter::test_read_smirnoff_spec_freesolv[0_1-2518989-0_0_2-False]
15.15s call     openff/toolkit/_tests/test_forcefield.py::TestSmirnoffVersionConverter::test_read_smirnoff_spec_freesolv[0_1-7754849-0_0_2-False]
13.64s call     openff/toolkit/_tests/test_molecule.py::TestMolecule::test_qcschema_molecule_record_round_trip_from_to_from
13.16s call     openff/toolkit/_tests/test_forcefield.py::TestForceField::test_parameterize_large_system[propane_methane_butanol_0.2_0.3_0.5.pdb-toolkit_registry1]
12.80s call     openff/toolkit/_tests/test_forcefield.py::TestForceField::test_parameterize_large_system[propane_methane_butanol_0.2_0.3_0.5.pdb-toolkit_registry0]
12.55s call     openff/toolkit/_tests/test_forcefield.py::TestSmirnoffVersionConverter::test_read_smirnoff_spec_freesolv[0_1-4936555-0_0_2-False]
12.54s call     openff/toolkit/_tests/test_forcefield.py::TestSmirnoffVersionConverter::test_read_smirnoff_spec_freesolv[0_1-8916409-0_0_2-False]
12.01s call     openff/toolkit/_tests/test_forcefield.py::TestSmirnoffVersionConverter::test_read_smirnoff_spec_freesolv[0_1-3201701-0_0_2-False]
11.18s call     openff/toolkit/_tests/test_forcefield.py::TestSmirnoffVersionConverter::test_read_smirnoff_spec_freesolv[0_1-7829[570](https://github.com/openforcefield/openff-toolkit/actions/runs/7573450245/job/20625618602#step:16:571)-0_0_2-True]
11.10s call     openff/toolkit/_tests/test_forcefield.py::TestForceField::test_parameterize_large_system[cyclohexane_ethanol_0.4_0.6.pdb-toolkit_registry0]
10.99s call     openff/toolkit/_tests/test_forcefield.py::TestForceField::test_get_available_force_fields_loadable[ff14sb_off_impropers_0.0.1.offxml-full_path0]
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d8e1d7a51fdd`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.