# Issue

**Title:** Add loss out header

## Description

## Summary

`nep` now writes a header to `loss.out` in the style of `thermo.out`. The header holds `# format_version 1`, `# output_interval N` and `# columns ...`. The column names depend on the model. A standard potential or TNEP model has `generation total L1 L2` followed by the energy, force and virial RMSE for the training and test sets. A charge model (`charge_mode` or `charge_vdw`) adds `rmse_charge_*` and `rmse_bec_*`. A dipole or polarizability model has `rmse_dipole_*` or `rmse_polarizability_*`. The header is written in the `Fitness` constructor, before any row, and only in training mode. A restarted run appends a further header.

This MR adds the column names to the file, so a reader such as calorine can take them from the header instead of inferring the layout from the number of columns and the model type. It also revises `doc/nep/output_files/loss_out.rst` to use the column names from the header, and adds the charge-model columns. The `loss.out` file of `gnep` is unchanged.

## Breaking changes

A reader that does not skip `#` lines fails on the header. The MATLAB scripts `examples/nep_train/plot_results.m` and `examples/qnep_train/plot_results.m` read the file with `load loss.out`, which is expected to fail on the header. This was not tested since neither MATLAB nor Octave was available. `np.loadtxt` skips the header by default.

## Testing

Short training runs with `nep`, built with `-arch=sm_120`, checked that the number and order of the column names match the fields of each row. The runs covered the standard potential, TNEP, `charge_mode`, `charge_vdw`, vdW only, dipole and polarizability models (10, 14 or 6 columns).
A run with `output_interval 2` wrote rows at generations 2, 4 and 6.
A restarted run appended a second header.
A run with `prediction 1` wrote no `loss.out`.
`git clang-format` made no changes to the edited lines.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `87fcc0056441`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.