# Issue

**Title:** Improve nonbonded decoupling protocol for single topology transformations

## Description

The schedule used to determine the 4-d coordinate offset for nonbonded decoupling in single topology calculations is currently suboptimal in a way that is easy to fix.

Brief background:
* Before #893, decoupling of dummy atoms was controlled by a single alchemical schedule `w(lam)`. The 4-d coordinates for dummy atoms were then determined by `w_a(lam) = w(lam)` for dummy atoms in A, or `w_b(lam) = cutoff - w(lam)` for dummy atoms in B. The latter functional form was hardcoded.
* Hence, the single alchemical schedule was constrained to be symmetric (e.g. needed to "slow" near $\lambda=0$, where dummy atoms in A decouple, and also "slow" near $\lambda=1$, where dummy atoms in B couple).
* Since #893, we no longer have this functional form imposed on us. In #900 we opted to just maintain the status quo, using the existing symmetric pre-optimized schedule and reproducing the same functional form for `w_a` and `w_b` (even though there was no requirement for `w_a` and `w_b` to each be symmetric).

This PR modifies the existing 4-d decoupling schedule to be more gradual, taking advantage of the fact that `w_a` and `w_b` no longer need to be individually symmetric. The modification was done by taking the relevant half of the existing pre-optimized schedule depending on whether the atom in question is being coupled or decoupled.

![decoupling_protocol](https://github.com/proteneer/timemachine/assets/319411/886403ab-7ece-46de-8012-f89bb85f5980)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b64ebd654617`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.