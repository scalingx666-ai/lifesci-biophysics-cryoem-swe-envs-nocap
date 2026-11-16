# Issue

**Title:** fix ufm params

## Description

This pull request updates the `run_integration` method in `calphy/liquid.py` to improve flexibility and accuracy in defining simulation parameters. The changes primarily focus on making the `ufm` (Uhlenbeck-Ford model) parameters configurable through class attributes instead of hardcoding them.

Key changes to simulation parameter configuration:

* Updated the `pair_style` command to use the `ufm_cutoff` attribute instead of a hardcoded value [[1]](diffhunk://#diff-57754aa82b61708d302240d2f417fe4ab70b0c8b20b27b5f7d2fb47337e1153eL247-R248) [[2]](diffhunk://#diff-57754aa82b61708d302240d2f417fe4ab70b0c8b20b27b5f7d2fb47337e1153eL344-R348).
* Modified the `pair_coeff` command to include the `sigma` parameter from the `uhlenbeck_ford_model` attribute, making it more flexible and aligned with the model's configuration.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7f2cc246c680`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.