# Issue

**Title:** Adding vector feature cutoff to ET

## Description

The user in #254 discovered a discontinuity in the energy at the cutoff when using ET.
We found that this is caused by a quirk in the current architecture that cannot be avoided without invalidating previous checkpoints.

The message passing step in the current architecture, which is consistent with eq. 9 in [1], weights only scalar features with the cutoff function, not vector ones. Given that both distance projections (eq. 5) have a bias, this allows the arch to learn a non-vanishing offset on the energy.

We can solve this by modifying the message passing so that the cutoff also affects vector features. This amounts to taking away the $\phi(d_{ij})$ from eq. 8 and multiplying it instead at eq.9a, so that $s^1_{ij}, s^2_{ij}, s^3_{ij} = \text{split}(V_j\odot D^V_{ij}) \phi(d_{ij})$.

Since this invalidates checkpoints I am gating this change behind a new option, `vector_cutoff`, that defaults to false and if turned on implements this new behavior.

Enabling this option seems to slightly improve benchmarks, for instance, this is ET-MD17.yaml with and without (reference) vector_cutoff. Note that I did not do any kind of hparam optimization.
![image](https://github.com/torchmd/torchmd-net/assets/13015792/df8d7f96-1272-4f77-9f63-b98083d22ed9)

Moreover, since we are moving away from ET in general in favor of TensorNet, I am using this PR to mark ET as deprecated.


[1] https://arxiv.org/pdf/2202.02541.pdf

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8e113eb78908`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.