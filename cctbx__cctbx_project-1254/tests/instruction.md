# Issue

**Title:** iotbx, xcif: declare the xcif dependency in libtbx_config

## Description

## Problem

`iotbx.cif` has defaulted to the xcif parser engine since d971ea2c4e and imports `xcif_ext` on every default CIF read, but `iotbx/libtbx_config` never listed `xcif` as a dependency. Builds that run `libtbx.configure iotbx` (or any module that depends on iotbx) without naming `xcif` explicitly therefore never configured or built `xcif_ext`, and tests fail with an import error. Our own CI and bootstrap did not catch this because they pass `module=xcif` explicitly.

## Changes

- `iotbx/libtbx_config`: add `xcif` to `modules_required_for_use`.
- `xcif/libtbx_config` (new): declare `cctbx` as required for use and build. `xcif/__init__.py` imports `cctbx.array_family.flex`, and `xcif/SConscript` needs `env_etc.scitbx_include` and `env_etc.boost_adaptbx_include`, which only exist once those modules are configured ahead of it.

## Verification

Fresh configure in a temporary directory with `conda_base/bin/python .../libtbx/configure.py --use_conda <module>`:

| Command | `xcif` in module list | `xcif/SConscript` in SConstruct |
|---|---|---|
| `configure iotbx` before | no | no |
| `configure iotbx` after | yes (between `ucif` and `smtbx`) | yes |
| `configure xcif` after | yes (after `libtbx ... scitbx cctbx`) | yes |

Configure-time change only; no compile was run.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b09bff6ed550`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.