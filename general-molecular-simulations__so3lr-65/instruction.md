# Issue

**Title:** Update vulnerable dependencies

## Description

Updates the locked dependencies to address all 63 current Dependabot alerts across 11 packages, including 41 Pillow advisories. The targeted resolution changes 15 package versions and preserves JAX/JAXlib, Flax, Optax, Orbax, NumPy, and every Git dependency revision.

The `tfds` extra moves to TensorFlow/TensorBoard 2.21 and requires patched setuptools. TensorBoard 2.20 imports the removed `pkg_resources` module, so upgrading setuptools alone breaks startup. TensorBoard is now explicit because TensorFlow 2.21 no longer installs it automatically. TensorFlow's declared h5py constraint moves the universal lock from h5py 3.16 to 3.14; HDF5 output and data-loading tests cover that change.

Dependabot uses the uv ecosystem with weekly version checks and grouped security updates. The PR changes only `pyproject.toml`, `uv.lock`, and `.github/dependabot.yml`.

### MeluXina validation

Validated the dependency configuration from commit `321102f51285bb2e6b33f472ee615f55b1733ff4` in fresh locked environments with uv 0.11.26. The dependency files are identical in this PR. Source checksums were verified. The recorded counts below include one-off compatibility checks used during validation; the final PR adds no test files or CI jobs.

| Run | Result |
| --- | --- |
| Default CPU suite, Python 3.12.13 | 796 passed, 100 skipped, 1 pre-existing logger failure |
| Default CPU suite, Python 3.13.2 | 796 passed, 100 skipped, 1 pre-existing logger failure |
| One-off compatibility checks, Python 3.12.13 / 3.13.2 | 5 passed on each |
| Full selected TFDS, MLFF, training, fine-tuning and HDF5 tests, Python 3.12.13 | 253 passed |
| Full selected optional tests, Python 3.13.2 | 253 passed |
| A100 GPU model/regression tests, Python 3.12.13 | 17 passed, 3 expected skips for absent optional tools |

The sole broad-suite failure is `test_fit_early_stopping` capturing its log twice after CLI error tests, already described in #52. A separate baseline job reproduced the same failure on unchanged `main` with its original lockfile and on this candidate; the candidate's test passes in isolation. No new test failures were found.

Slurm jobs: CPU array `5444889`, GPU `5444890`, baseline comparison `5445103`. All 63 live advisory ranges were checked against the candidate lock; none still match. Alerts will close after this reaches the default branch. #63's JAX changes remain separate.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c892238b75d2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.