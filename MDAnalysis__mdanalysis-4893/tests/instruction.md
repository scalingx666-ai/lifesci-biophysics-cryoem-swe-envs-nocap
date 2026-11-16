# Issue

**Title:** Test on smaller pdb for guesser to reduce test duration

## Description

Changes made in this Pull Request:
 - use a trimmed version of `PDB_small` for guesser tests.

Reduce from

============================= slowest 50 durations =============================
93.06s call     MDAnalysisTests/guesser/test_base.py::TestBaseGuesser::test_guessing_angles_respects_bond_kwargs
40.04s call     MDAnalysisTests/analysis/test_base.py::test_AnalysisFromFunction[client_AnalysisFromFunction1-20-50-2-15]
39.70s call     MDAnalysisTests/analysis/test_base.py::test_AnalysisFromFunction[client_AnalysisFromFunction1-20-50-None-30]
39.64s call     MDAnalysisTests/analysis/test_base.py::test_AnalysisFromFunction[client_AnalysisFromFunction1-None-50-2-25]
38.50s call     MDAnalysisTests/analysis/test_base.py::test_AnalysisFromFunction[client_AnalysisFromFunction1-None-None-2-49]
30.85s call     MDAnalysisTests/analysis/test_rms.py::TestRMSD::test_rmsd_atomgroup_selections[client_RMSD1]
30.65s call     MDAnalysisTests/analysis/test_rms.py::TestRMSD::test_weights_mass_is_mass_weighted[client_RMSD1]
28.09s call     MDAnalysisTests/guesser/test_base.py::TestBaseGuesser::test_guess_topology_objects_out_of_order_guess
26.96s call     MDAnalysisTests/analysis/test_contacts.py::TestContacts::test_contacts_selections[client_Contacts1]
26.27s call     MDAnalysisTests/guesser/test_base.py::TestBaseGuesser::test_guess_topology_objects_out_of_order_init
23.74s setup    MDAnalysisTests/analysis/test_bat.py::TestBAT::test_bat_IO[client_BAT1]

to 

============================= slowest 50 durations =============================
39.72s call     MDAnalysisTests/analysis/test_base.py::test_AnalysisFromFunction[client_AnalysisFromFunction1-20-50-2-15]
39.54s call     MDAnalysisTests/analysis/test_base.py::test_AnalysisFromFunction[client_AnalysisFromFunction1-20-50-None-30]
39.50s call     MDAnalysisTests/analysis/test_base.py::test_AnalysisFromFunction[client_AnalysisFromFunction1-None-50-2-25]
39.38s call     MDAnalysisTests/analysis/test_base.py::test_AnalysisFromFunction[client_AnalysisFromFunction1-None-None-2-49]
29.26s call     MDAnalysisTests/analysis/test_rms.py::TestRMSD::test_rmsd_atomgroup_selections[client_RMSD1]
29.25s call     MDAnalysisTests/analysis/test_rms.py::TestRMSD::test_weights_mass_is_mass_weighted[client_RMSD1]
26.90s call     MDAnalysisTests/analysis/test_contacts.py::TestContacts::test_contacts_selections[client_Contacts1]
23.60s setup    MDAnalysisTests/analysis/test_bat.py::TestBAT::test_bat_IO[client_BAT1]
19.68s call     MDAnalysisTests/guesser/test_default_guesser.py::test_aromaticity
17.90s call     MDAnalysisTests/analysis/test_rms.py::TestRMSD::test_ref_mobile_mass_mismapped[client_RMSD1]
17.79s call     MDAnalysisTests/coordinates/test_pdb.py::test_conect_bonds_all

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d22272083967`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.