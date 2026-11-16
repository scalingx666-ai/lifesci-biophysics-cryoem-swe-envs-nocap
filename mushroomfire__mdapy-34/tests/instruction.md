# Issue

**Title:** MDAPY 2.0.0

## Description

The release of 2.0.0, merged on the day of the release together with the version number.

- `pip install mdapy` without `--pre` in the README, the home page and Installation; a line for code written for 1.x.
- PyPI classifier: Production/Stable.
- Release notes of 2.0.0: an introduction, and what changed since 2.0.0rc1 (pip without `--pre`, the systems officially supported, the options of `plot.style`, a fix of one's own in `MolecularDynamics.run`, pictures from far away without rings).
- The API snapshot `tests/api/2.0.0.txt` holds the options of `plot.style` added since rc1.

Left for the day of the release: the version number in `src/mdapy/__init__.py`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f83b005fef88`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.