# Issue

**Title:** Raise NEP descriptor and single-layer neuron limits

## Description

**Changes**
- `MAX_DIM` 103 to 150, `MAX_DIM_ANGULAR` 90 to 135.
- `nep.in` and the nep.txt parser and converter: `n_max` up to 15 14, angular `basis_size` up to 16, one hidden layer up to 300 neurons (two layers stay at 120).
- Dimension checks use `MAX_DIM` and `MAX_DIM_ANGULAR` instead of hard-coded numbers.
- Spin models keep their current limits, since their model file and `MAX_DIM_SPIN` are unchanged.

**Bug fix**
`gpumd` reads legacy nep.txt files (`force/nep.cu`, `force/nep_multigpu.cu`) without checking `n_max`, `basis_size` or the descriptor dimension against the per-thread array sizes.
A model that is too large, for example one written by an external trainer, overruns those arrays silently: on master it runs without any error but gives garbage energies, forces and MD.
The readers now stop with an error.

**Why**
Gradient-based trainers such as torchNEP and NEPPO train large NEP models well, and their error keeps falling as the descriptor and hidden layer grow.
In some cases this brings NEP close to MACE accuracy at a small fraction of the cost.
For a hybrid perovskite dataset (710 structures) the best model inside the current limits reaches F 35.2 meV/Å and E 0.51 meV/atom (81k parameters).
With `n_max 14 14`, `basis_size 14 14` and 250 neurons it reaches F 19.9 meV/Å and E 0.41 meV/atom (244k parameters), and runs at 1.8e6 atom-steps/s on one GH200 against 3.2e6 for the 81k model.

![rmse vs params](https://raw.githubusercontent.com/erikfransson/GPUMD/pr-figures/rmse_vs_params_nep_mace.png)
Train force (a, b) and energy (c, d) RMSE on the 710 structures against parameter count, NEP left and MACE right.
NEP colour is the radial cutoff and size the angular cutoff, faint points are beyond the current GPUMD limits.
MACE colour is the cutoff and size the number of interaction layers.
Open black markers are SNES models, stars the best NEP inside (red) and beyond (blue) the current limits.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `fa6c96a975d5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.