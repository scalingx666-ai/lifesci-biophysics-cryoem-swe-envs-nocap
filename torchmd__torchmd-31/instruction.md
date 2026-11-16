# Issue

**Title:** MD starting from npz file

## Description

These are a set of modifications to use a file.npz to run a molecular dynamics:
- for a molecule you can build a file.npz in which the tag for the position is "coord" and the tag for the atomic-numbers is "z".
- **npzMol.py** --> new class that take in input a npz file and create an object with the same attributes of Molecule from moleculekit;
- **run.py** --> new `parser.add_argument(--npz_file)`, in the conf.yaml you can specify the path of the npz file to use it as input (topology and structure should be null in the conf.yaml);
- **run.py**  --> `batch_comp ` is a flag that becomes true when you use the npz file as input to run the dynamic, and it is used to directly create the embedding from the 'z';
- **utils.py** --> converter from npy to xyz created, this is useful if you want to open the trajectory in software for molecular visualization like Pymol, Avogadro, etc.; this function is then called in the run.py

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5090627e1cf4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.