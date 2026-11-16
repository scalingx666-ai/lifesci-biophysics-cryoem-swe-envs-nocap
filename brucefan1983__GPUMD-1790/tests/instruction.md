# Issue

**Title:** Add thermodynamic integration ensemble for NEP (ti_nep)

## Description

### Summary

This PR adds a new ensemble, `ti_nep`, for nonequilibrium thermodynamic integration between two NEP potentials. It computes the free-energy difference between two systems described by different potentials, sampled on the same atomic configuration.

This was developed to use in calculations of finite-temperature free-energy differences between charge states of point defects (charge transition levels), where one NEP model describes the different charge states through species labels (see [this paper](https://arxiv.org/pdf/2512.15463)). These differences are small compared with absolute free energies. Switching directly between the two potentials gives much lower statistical error than computing each state separately with Frenkel–Ladd (`ti_spring`) and subtracting. The method can also be used for other alchemical-type transformations between two NEP descriptions of a system.

## Changes

This PR adds a new ensemble, `ti_nep`. It computes the Helmholtz free-energy difference between two NEP potentials, F(NEP1) − F(NEP2), by nonequilibrium thermodynamic integration. The atoms are coupled to a Langevin thermostat while the Hamiltonian is switched as

U(λ) = (1 − λ) U_NEP1 + λ U_NEP2


### New files
- `src/integrate/ensemble_ti_nep.cuh`, `src/integrate/ensemble_ti_nep.cu`: class `Ensemble_TI_Nep`, derived from `Ensemble_LAN`.
  - The first `potential` in `run.in` (λ = 0) does the regular force calculation, as in the default `observe` mode for multiple potentials. At every step the ensemble also evaluates the second potential (λ = 1) on the same configuration, and the forces are mixed as (1 − λ) F₁ + λ F₂ before the Langevin integration step.
  - The protocol follows `ti_spring`: equilibration at λ = 0 (`tequil`), forward switch 0 → 1 (`tswitch`), equilibration at λ = 1, then backward e polynomial switching function as`ti_spring`. F_diff is the average of the forward and backward work, which cancels dissipation to first order.
  - If `tequil`/`tswitch` are omitted, they are set automatically to 10 % and 40 % of the run length, as in `ti_spring`.

### Modified files
- `src/integrate/ensemble.cuh`: adds `ti_nep`
- `src/integrate/integrate.cu`: parses `ensemble ti_nep` and creates the new ensemble.

## Validation: `ti_nep` vs `ti_spring` (Frenkel–Ladd) for the O vacancy in MgO

### Test case
The test quantity is the Helmholtz free-energy difference between two charge states (+0 and +2) of an oxygen vacancy in MgO:

ΔF = F(+0) − F(+2)  (total energy of the cell, eV)

The two charge states are described by one NEP model. They differ only in the species label of the 6 Mg atoms next to the vacancy (`Pb` = +0, `I` = +2), as in [this paper](https://arxiv.org/pdf/2512.15463). The `ti_nep` result is compared with Frenkel–Ladd (`ti_spring`) free energies calculated separately for each state. The two methods use the same cells, so their ΔF values are the same quantity and should agree within statistical error.

### Simulation details
Common settings: 1 fs time step. Temperatures are 1, 250, 500, 750 and 1000 K.

1. **Pristine NPT (P = 0):** 13824-atom MgO supercell (12×12×12 conventional cells), `npt_scr` for 50 ps at each T. The last 25 ps (50 frames) are averaged into a mean structure.
2. **Defect cells:** in the averaged structure, the O atom closest to the cell centre is removed and its 6 nearest Mg neighbours are relabelled. This gives 13823-atom cells for +0 and +2, both at the pristine zero-pressure volume.
3. **`ti_spring`:** for each state and T, 25 independent runs, each consisting of
   ensemble  nvt_lan T T 100                      # 20 ps
   ensemble  ti_spring temp T tperiod 100 tequil 5000 tswitch 100000 spring O 2.0 Mg 2.0 Pb(or I) 2.0   # 210 ps
ΔF = N·(F₊₀ − F₊₂).
4. **`ti_nep`:** for each T, 5 independent runs in the +0 cell. λ = 0 is the +0 NEP; λ = 1 is the same NEP with the `Pb`/`I` labels swapped, so the defect neighbours are evaluated as +2.
   potential nep.txt            # λ = 0
   potential nep_inverted.txt   # λ = 1
   ensemble  nvt_lan T T 100                                     # 10 ps
   ensemble  ti_nep temp T tperiod 100 tequil 500 tswitch 2500   # 6 ps
ΔF = N·F_diff (from `ti_nep.yaml`).

### Results

<img width="1198" height="429" alt="image" src="https://github.com/user-attachments/assets/5f07ba60-f425-46cc-9149-c44dde60669d" />

Both `ti_nep` and `ti_spring` indicate a shift downward with temperature. 
The 5 independent `ti_nep` runs give  a small error bars and are well converged at these simulations settings. 
The 25 independent `ti_spring` runs have larger error bars, but are within one STD of the `ti_nep` averages. 
Single `ti_nep` runs with a longer switch times on an NPT-equilibrated +0 cell of the same size, from [this paper](https://arxiv.org/pdf/2512.15463), is included in this plot.

The run-to-run scatter of the independent runs can be seen below. Note that for `ti_spring`, we have two cases: "+0" and "+2".
This is due to the method calculating the absolute free energy of both these charge states and then obtain the formation free energy from the difference of these. 
The run-to-run scatter is much lower for the `ti_nep` case, most likely due to the very small energy difference related to changing charge state, which is why we use this method for this type of problem.

<img width="1638" height="407" alt="image" src="https://github.com/user-attachments/assets/a627e7a3-69f9-45fb-9b07-28d5aa1c5eb9" />

Overall: `ti_nep` performs better than `ti_spring` in this type of problem.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `87fcc0056441`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.