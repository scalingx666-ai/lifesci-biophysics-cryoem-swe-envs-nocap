# Issue

**Title:** Fix long-range spin dipole energy, forces, and pressure (issue #4717)

## Description

**Summary**

Fix long-range spin dipole energy, forces, and pressure (issue #4717)

**Related Issue(s)**



**Author(s)**

Stan Moore (SNL) and Claude Code (Opus 4.8)

**Licensing**

By submitting this pull request, I agree, that my contribution will be included in LAMMPS and redistributed under either the GNU General Public License version 2 (GPL v2) or the GNU Lesser General Public License version 2.1 (LGPL v2.1).

**Artificial Intelligence (AI) Tools Usage**

<!--Please update the following sentence as needed in case you were using AI tools to
generate code.  Please mention which specific AI tool or tools you were using and which
parts of the code were AI generated versus the parts written by a human.  The use of AI
tools for checking spelling and syntax does not need to be reported.-->

I used Claude Code (Opus 4.8)

**Backward Compatibility**

Yes

**Implementation Notes**

Description from Claude:

Fixes the `spin/dipole/long` pair style and its `ewald/dipole/spin` and `pppm/dipole/spin` kspace partners, which gave dipolar energies that disagreed with `spin/dipole/cut` and the analytical result, and were not independent of the real-space cutoff when shifting work between the short- and long-range parts (LAMMPS issue #4717).

## Real-space pair (`pair_spin_dipole_long`)
- **`compute_long`**: the screened dipolar field is `B2*r^2*(sj.eij)*eij - B1*sj`. The `r^2` (`rsq`) factor on the `B2` term was missing, which corrupted both the precession field and the energy (`evdwl = -0.5*hbar*sp.fmi`). With the fix this reduces to `compute_dipolar` in the cut style as `g_ewald -> 0`. This is the primary cause of the reported wrong energy.
- **`compute_long_mech`** was accumulated into `fmi` instead of `fi` (so there were no real-space mechanical forces at all), and was missing the `r^2` factor on the `(si.eij)(sj.eij)` term, the overall factor of `r`, and the sign. Both helpers now take `rsq`.
- The **pair virial** was tallied with `rij = x_j - x_i` instead of `x_i - x_j`, giving the wrong sign (attractive pairs reported positive pressure) and preventing cancellation against the reciprocal-space virial. The same one-line sign fix is applied to `spin/dipole/cut`.
- `write_restart`/`read_restart` wrote/read the `double` cutoff with `sizeof(int)`.

## Reciprocal space (`ewald_dipole_spin`, `pppm_dipole_spin`)
- `fm_long` (the kspace precession contribution) was missing the per-atom spin norm `sp[i][3]`; ewald additionally had the wrong sign. The slab corrections used `spscale` instead of `spscale2` and also dropped `sp[i][3]`.
- `mu_0` was `785.15` in the kspace styles but `784.15` in the pair styles, so the real- and reciprocal-space prefactors did not match.
- `mu2` used in the `g_ewald` estimate dropped the `mub2mu0` prefactor.

## Verification
Built with SPIN + KSPACE (+ DIPOLE for cross-checks) and confirmed on the issue reproducer and periodic spin lattices that:
- the total energy, mechanical forces, and pressure are independent of the real-space cutoff and of `g_ewald`;
- `spin/dipole/long` now matches `spin/dipole/cut` and the analytical dipolar energy (including the −2:+1 ratio between parallel and perpendicular configurations);
- the 2-spin attractive case now gives negative pressure (was positive);
- `pppm/dipole/spin` converges to `ewald/dipole/spin`.

A spin port of the dipole RMS force-accuracy test confirms the achieved force error is cutoff-independent and tracks the requested kspace accuracy across `accuracy ∈ {1e-3, 1e-4, 1e-5}` and `cutoff ∈ {3, 5, 7, 10}` (test input posted as a comment below).

Adapted test from Claude:

### Spin long-range RMS force-accuracy test (port of the `ewald/dipole` test from issue #4717)

100 random atoms (min separation 2 Å) in a 24³ Å³ periodic box. A converged reference (`accuracy 1e-10`, `cutoff 10`) defines the "true" forces; each `(accuracy, cutoff)` run is compared against it. `fix nve/spin lattice moving` exercises both the real-space and reciprocal-space mechanical forces. Sweep with `-var acc {1e-3,1e-4,1e-5} -var cut {3,5,7,10}`.

```lammps
# Spin analog of the ewald/dipole RMS force-accuracy test (LAMMPS issue #4717)
# usage: lmp -in spin_rms_force_accuracy.in -var acc 1.0e-5 -var cut 5
units           metal
atom_style      spin
boundary        p p p
atom_modify     map array

# 100 random positions, min separation 2 Ang, box large enough for cut=10
region          box block 0 24 0 24 0 24 units box
create_box      1 box
create_atoms    1 random 100 123456 box overlap 2.0 maxtry 200
mass            1 55.845
set             group all spin/random 98934 1000.0   # large norm -> measurable forces

neighbor        2.0 bin
neigh_modify    every 1 check yes delay 0

# ---- reference: tight accuracy, large cutoff ----
pair_style      spin/dipole/long 10.0
pair_coeff      * * 10.0
kspace_style    ewald/dipole/spin 1.0e-10
kspace_modify   compute yes
fix             1 all nve/spin lattice moving
run             0
fix             ref all store/state 0 fx fy fz       # freeze reference forces

# ---- test: requested accuracy and cutoff ----
pair_style      spin/dipole/long ${cut}
pair_coeff      * * ${cut}
kspace_style    ewald/dipole/spin ${acc}
kspace_modify   compute yes
run             0

# RMS force deviation from the converged reference
variable        dfsq atom ((fx-f_ref[1])^2+(fy-f_ref[2])^2+(fz-f_ref[3])^2)
variable        rfsq atom (f_ref[1]^2+f_ref[2]^2+f_ref[3]^2)
compute         cd all reduce ave v_dfsq
compute         cr all reduce ave v_rfsq
variable        absrms equal sqrt(c_cd)
variable        relrms equal sqrt(c_cd/c_cr)
run             0
print           "RESULT acc=${acc} cut=${cut} absrms=$(v_absrms:%.6e) relrms=$(v_relrms:%.6e) reffrms=$(sqrt(c_cr):%.6e)"
```

Sweep driver:

```bash
for acc in 1.0e-3 1.0e-4 1.0e-5; do
  for cut in 3 5 7 10; do
    lmp -in spin_rms_force_accuracy.in -var acc $acc -var cut $cut 2>/dev/null | grep "^RESULT"
  done
done
```

**Relative RMS force error** (‖Δf‖ / ‖f_ref‖, reference RMS force ≈ 5.5 eV/Å, using 200 atoms for denser statistics):

| accuracy \ cutoff | 3 | 5 | 7 | 10 |
|---|---|---|---|---|
| **1e-3** | 2.94e-3 | 2.92e-3 | 2.81e-3 | 2.75e-3 |
| **1e-4** | 2.95e-4 | 2.75e-4 | 3.32e-4 | 2.50e-4 |
| **1e-5** | 2.81e-5 | 3.03e-5 | 2.66e-5 | 2.56e-5 |

Across each row the error is flat over cutoffs 3→10 (cutoff-independent: work shifts between short and long range but the forces are the same), and down each column it drops ~10× per decade of requested accuracy. The large spin norm just scales the forces up so the relative error is physically meaningful; the cutoff-independence holds regardless.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `697545ba8b25`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.