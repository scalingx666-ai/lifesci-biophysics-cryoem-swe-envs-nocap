# Issue

**Title:** Generalize deform and decouple it from NPT ensembles

## Description

# **Summary** 

This PR makes deform an independent action instead of an operation embedded in NPT-Berendsen/NPT-SCR, and extends it to general triclinic box deformation.

# **Usage**

## Grammar

Old syntax remains supported:

```
deform 0.0001 1 0 0
```

```
deform 0.0001 0.0002 0.0003 1 1 1
```

New general syntax:

```
deform xx 0.0001
```

```
deform xy 0.0001
```

```
deform xx 0.0001 yy -0.00005 xy 0.0002
```

All rates are box-matrix increments in A/step, consistent with the existing `deform` convention.

For the general syntax, `xy`, `xz`, and `yz` refer to the corresponding GPUMD box-matrix components. They are geometric box/tilt components, not symmetric Voigt shear-strain components.


## NPT compatibility

- NVE + deform: supported.
- Standard NVT + deform: supported.
- 3-component NPT + diagonal `xx/yy/zz` deform: supported for orthogonal boxes.
- 3-component NPT + shear deform: rejected; use 6 target pressure components.
- 6-component NPT + general deform: supported.
- Isotropic NPT + deform: rejected.
- Legacy deform syntax + 6-component NPT: rejected; the general syntax should be used instead.

## Scope / known limitation

* This PR does not implement automatic lattice reduction / box flipping for very large cumulative shear. The current implementation is intended for finite deformations where the triclinic cell does not become excessively skewed. This can be added later if a practical need arises.
* Deform + NPT-MTTK is not supported. Use Berendsen/SCR instead.


# **Validation** (Describe how this pull request was tested)

Existing features not using deform are unchanged. The legacy `deform` syntax remains supported with the same prescribed deformation behavior.

# **Licensing** (Please read and confirm before submitting this pull request)

By submitting this pull request, I agree that my contribution will be included in GPUMD and redistributed under the existing GPUMD license. Newly added source files follow the existing GPUMD license-header style when applicable. No external source code with incompatible licensing has been copied into this pull request.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `09a72d2bc81b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.