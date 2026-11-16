# Issue

**Title:** unsampled endstates long range correction as an input yaml option

## Description

## Description

This exposes the `unsampled_endstates` boolean option in the input YAML file for enabling/disabling the creation of unsampled endstates as a long-range steric correction.

## Motivation and context

<!--- Why is this change required? What problem does it solve? -->

<!-- Replace ??? with the issue number that this pull request resolves. -->
 

## How has this been tested?

Tested locally with protein-ligand cli example. Output of matrix as follows, for `n_states=3`. Dimensions of `3x3` vs `5x5` (Format is not great, sorry).

| `unsampled_endstates:False`  | `unsampled_endstates:True` |
| ------------- | ------------- |
| Deltaf_ij:<br>   0.000  20.018  -3.468<br> -20.018   0.000 -23.485<br> 3.468  23.485   0.000 | Deltaf_ij:<br>   0.000   7.158  21.007  -4.126  -9.088<br> -7.158   0.000  13.848 -11.284 -16.247<br> -21.007 -13.848   0.000 -25.133 -30.095<br>   4.126  11.284  25.133   0.000  -4.962<br>   9.088  16.247  30.095   4.962   0.000 |

## Change log

<!-- Propose a change log entry. -->
<!-- Examples here https://github.com/choderalab/perses/blob/master/docs/changelog.rst -->
```
``unsampled_endstates`` boolean option in input YAML file, for enabling/disabling creation of unsampled endstates long-range sterics correction.
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e81ae32d64da`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.