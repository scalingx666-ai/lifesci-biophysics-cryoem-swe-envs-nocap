# Issue

**Title:** Enable interfacial confinement (xy translations only, full 3D rotations) in HPMC

## Description

## Description

Adds `translate_move_dimensions` to `HPMCIntegrator`, allowing translation moves to be restricted to the xy-plane in 3D simulations while rotation moves remain fully three-dimensional. When set to 2 in a 3D box, no z-component of trial displacements. Runtime error is raised when unphysical setup is used (e.g. when the box is 2D and `translate_move_dimensions` is set to 3)

This revives the work started in #733 (closed as stale). The API follows the design we agreed on in the issue #2247.

## Motivation and context

<!--- Why is this change required? What problem does it solve? -->
It's possible to do quasi-2D without out-of-plane rotations now, but having objects with out-of-plane rotations would be nice for studying interfacially confined objects (such as work from Escobedo group Thapar, V., T. Hanrath, and F. A. Escobedo. Soft Matter (2015)).

This topic has also been raised many years ago (see ).
<!-- Replace ??? with the issue number that this pull request resolves. -->
 

## How has this been tested?

Added `hoomd/hpmc/pytest/test_translate_move_dimensions.py` with 7 test cases:

- `test_default_is_none`: Verifies that `translate_move_dimensions` defaults to `None`. W assume translation moves match the system dimensionality unless explicitly overridden.
- `test_set_and_get`: Sets `translate_move_dimensions` to 2 and 3 and reads the value back.

- `test_invalid_value_raises`: Passes nonsense or non-implemented dimensions 0, 1, 4, and -1 to the setter and confirms each raises `RuntimeError`, so users cannot silently run a simulation with a nonsensical dimensionality. Might need to be updated in the future (say, dimension 1).

- `test_z_positions_unchanged`: Runs translation-only steps with `translate_move_dimensions=2` and asserts all four particle z-coordinates are bit-for-bit identical before and afte to verify the C++ displacement mask in `IntegratorHPMCMono.h`.

- `test_xy_positions_do_change`: Runs the same setup and confirms xy coordinates do change.

- `test_rotations_active_with_dims_2`: Disables translation and runs rotation-only moves with `translate_move_dimensions=2`, confirming that rotation acceptance is nonzero and 3D rotations are unaffected by the translation constraint.

- `test_dims_3_allows_z_translation`: Runs with `translate_move_dimensions=3` and confirms z change, so the default 3D setup is not affected.

Let me know if these test cases are sufficient. 

I've also tried longer testings on my desktop and HPC. Looks fine to me.

<!--- Please build the sphinx documentation and check that any changes to
      documentation display properly. -->
I've built Sphinx documentation locally and added a new paragraph in the `Local trial moves` rubric and the `translate_move_dimensions` property (except for pure 2D system such assphere, convex polygon, simple polygon...). The docstring render correctly. I’m aiming to keep it consistent with how `translation_move_probability` is documented. It might be slightly repetitive, but it might be a good thing?

Thanks Josh for reviewing!

## Checklist:

- [x] I have reviewed the [**Contributor Guidelines**](https://github.com/glotzerlab/hoomd-blue/blob/trunk/CONTRIBUTING.rst).
- [x] I agree with the terms of the [**HOOMD-blue Contributor Agreement**](https://github.com/glotzerlab/hoomd-blue/blob/trunk/ContributorAgreement.md).
- [x] My name is on the list of contributors (`sphinx-doc/credits.rst`) in the pull request source branch.
- [x] I have summarized these changes in `CHANGELOG.rst` following the established format.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e0a8f6766695`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.