# Issue

**Title:** Embedding spin multiplicity and charge based on SpookyNet implementation

## Description

**Type of PR**
- [ x ] Feature


**Description**
Right now only singlet neutral charged molecules can be clearly distinguished by the model representations (SchNet, PainNN etc)
Main addition is the new class "ElectronicEmbedding". 
The implementation follows reference https://doi.org/10.1038/s41467-021-27504-0 (SpookyNet)
The new class can be invoked for SchNet and PaiNN representations (SO3 is not yet implemented)

**Validation of new Feature**
The feature was tested on two model systems Ag3+/Ag3- and singlet/triplet CH2. Training was done on energies and forces. The databases were the same as used in the reference. PES calculation were done on GFN2-xtb level of theory.
With spin_charge embedding deviations from **r** (bond length between Ag-Ag or C-H) and **$\alpha$** (angle between 2,0,1) are below 0.1 (see figure 1 and 2).
The RMSD for the minimum energy structures are below 10e-4 Angstrom.

![figure_2](https://github.com/epens94/schnetpack/assets/114085645/d2736d2d-d0c7-41d7-9bf4-afbe363be654)
![figure_1](https://github.com/epens94/schnetpack/assets/114085645/437a573b-fcae-4a50-a881-042263dcc55b)


When spin respective charge are embedded the qualitative correct PES can be obtained (see figure
![PES-carben](https://github.com/epens94/schnetpack/assets/114085645/d6886fd6-b770-433b-9cbb-5e34112f0862)
![PES-ag3](https://github.com/epens94/schnetpack/assets/114085645/2908743a-59c7-4cc7-b188-4410fb50ae82)



**Additional Notes**
In SpookyNet reference a complex nuclear embedding method based on the atomic electronic configuration was used. Invoking this nuclear embedding method results in slower model convergence (approx. ten times slower). The final error metric (MAE) is roughly 50% better than the simple nuclear embedding scheme. For both systems the MAEs on the test sets were 10-25x lower (Ag3+/Ag3-), 50-120x lower (singlet/triplet CH2) as plain chemical accuracy (1kcal/mol).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c3a4b30ee011`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.