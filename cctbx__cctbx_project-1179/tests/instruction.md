# Issue

**Title:** Add GFN-xTB QM engine (xtb_manager) to the quantum-restraints pipeline

## Description

New mmtbx.geometry_restraints.xtb_manager drives GFN-xTB constrained optimisations / single-point gradients through base_qm_manager. Defaults tuned for transition-metal sites: GFN2, ALPB(ether) solvent (~ protein interior), an inertial optimiser so $fix holds frozen scaffold atoms exactly, $constrain for held torsions, and a robust preset for hard SCFs. xtb is located via PHENIX_XTB (or the build bin) and each job is namespaced so many runs share a directory.

Wire xtb into the engine selection so it is usable via mmtbx.quantum_interface (program=xtb):
- quantum_interface: register is_xtb_installed/PHENIX_XTB and add the xtb branch to populate_qmr_defaults
- quantum_restraints_manager.get_qm_manager and qm_manager.qm_runner: dispatch program=xtb to xtb_manager
- base_qm_manager.process_qm_log_file: recognise xtb's "normal termination" (guarded against the "abnormal termination" substring)

Add tst_xtb_manager.py (input generation always; a small opt/gradient/torsion run when xtb is present, skipped otherwise) and register it in run_tests.py.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a0f80e3b817a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.