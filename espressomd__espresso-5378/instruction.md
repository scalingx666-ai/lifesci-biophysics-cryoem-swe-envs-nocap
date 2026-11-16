# Issue

**Title:** magnetostatics: require 'prefactor' for DipolarP3M

## Description

DipolarP3M.required_keys() returned only {"accuracy"}, so omitting the
prefactor silently passed the Python required-keys check. default_params()
then injected prefactor=0.0, the base float type check accepted it, and the
value reached the core, which threw the misleading
ValueError "Parameter 'prefactor' must be > 0" instead of the clean,
documented RuntimeError "Parameter 'prefactor' is missing".

Add 'prefactor' to DipolarP3M.required_keys() so the missing-key contract is
enforced at the Python layer before the placeholder default is applied. This
mirrors the established convention of electrostatics P3M
(required_keys() -> {"prefactor", "accuracy"}) and the sibling
DipolarDirectSum (required_keys() -> {"prefactor"}).

Extend test_exceptions_p3m in testsuite/python/dipolar_interface.py to assert
that constructing DipolarP3M without a prefactor raises the clean
RuntimeError "Parameter 'prefactor' is missing".

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c7298783c057`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.