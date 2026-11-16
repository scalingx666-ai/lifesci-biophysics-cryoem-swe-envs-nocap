# Issue

**Title:** specify input and output files by command-line arguments

## Description

I am submitting this PR so that you can see more clearly what changes I have made to packmol's source code in order to implement this feature.

The proposed feature allows users to specify the names of the input and output files via command-line arguments, while being backwards compatible.

It is proposed to execute packmol with command-line arguments is the following way:

```sh
packmol -in input.txt -out output.txt
```

Users may still use packmol with input redirection if they wish to do so:

```sh
packmol < input.txt
```

in that case the name of the output file must be present in `input.txt` by providing the respective `output` keyword and value pair.

I shall add the tests that you have requested to test the interface automatically via CI.

closes

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6dee06d48dd1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.