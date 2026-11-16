# Issue

**Title:** Fix issue #225

## Description

This PR , by fixing the `custom_parameters` keyword argument for AMBER protein force fields. This option was added when decoupling from the additional `leap_commands` option, which now allows the user to reference the loaded molecule. Importantly, `custom_parameters` must be placed at the start of the LEaP script, i.e. before the molecule is loaded. The option should be passed as a list of paths to custom parameter files, which will be loaded with `loadAmberParams`, or the names of additional internal `leaprc` files, e.g. `leaprc.gaff`, which will be sourced with `source`. I've added a test that validates that both types of file can be handled and that the output in the `LEaP` script is in the correct order.

* I confirm that I have merged the latest version of `devel` into this branch before issuing this pull request (e.g. by running `git pull origin devel`): [y]
* I confirm that I have permission to release this code under the GPL3 license: [y]

## Suggested reviewers:
@xiki-tempula

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d86dc9f94098`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.