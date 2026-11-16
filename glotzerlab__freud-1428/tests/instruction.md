# Issue

**Title:** Remove unnecessary allocations in Nematic OP

## Description

<!-- Provide a general summary of your changes in the Title above. -->

## Description

The nematic OP used Managedarray{3, 3} which is a vector backed data structure. This resulted in an unnecessary allocation in the innermost loop. Worse yet, the fancy multi-index of managed arrays allocates an extra vector for every call to `operator()`, so each particle was performing a huge number of unnecessary allocations. This PR swaps to using a basic `float[9]` as the intermediate buffer, which is then distributed to the particle and global tensors as needed.

A future PR revising the ManagedArray indexing is probably in order too -- . This PR is a separate change though, as nematic is so fast that the single managed array allocation quarters the performance.

## Motivation and Context

Benchmarks run on battery power on an M1 Pro MacBook.

```
BEFORE:
  scenario                            median     per 1k         min         max     order
  --------------------------------------------------------------------------------------
  uniform random                  med  43.230 ms  ( 432.30 us/k)  min  41.262  max  52.311  OP=0.002568
  perturbed theta=5 deg           med  40.345 ms  ( 403.45 us/k)  min  39.501  max  46.995  OP=0.988540
  perturbed theta=15 deg          med  40.231 ms  ( 402.31 us/k)  min  39.558  max  49.638  OP=0.899437
  perturbed theta=30 deg          med  40.669 ms  ( 406.69 us/k)  min  39.564  max  76.491  OP=0.625012
  50/50 mixed (theta=5)           med  43.379 ms  ( 433.79 us/k)  min  39.575  max 121.277  OP=0.494433
  50/50 mixed (theta=30)          med  40.859 ms  ( 408.59 us/k)  min  39.738  max 107.235  OP=0.313669
  10/90 mixed (theta=5)           med  40.122 ms  ( 401.22 us/k)  min  39.511  max  48.554  OP=0.098905


AFTER:

  scenario                            median     per 1k         min         max     order
  --------------------------------------------------------------------------------------
  uniform random                  med   1.177 ms  (  11.77 us/k)  min   1.074  max   1.726  OP=0.002568
  perturbed theta=5 deg           med   1.170 ms  (  11.70 us/k)  min   1.090  max   1.406  OP=0.988483
  perturbed theta=15 deg          med   1.168 ms  (  11.68 us/k)  min   1.092  max   1.400  OP=0.899524
  perturbed theta=30 deg          med   1.166 ms  (  11.66 us/k)  min   1.062  max   1.386  OP=0.625011
  50/50 mixed (theta=5)           med   1.169 ms  (  11.69 us/k)  min   1.095  max   1.408  OP=0.494429
  50/50 mixed (theta=30)          med   1.167 ms  (  11.67 us/k)  min   1.094  max   1.428  OP=0.313670
  10/90 mixed (theta=5)           med   1.168 ms  (  11.68 us/k)  min   1.098  max   1.424  OP=0.098907
```

## How Has This Been Tested?
<!-- Please describe in detail how you tested your changes. -->
<!-- Include details of your testing environment, and the tests you ran to
     see how your changes affect other areas of the code, etc. -->

## Checklist:
- [ ] I have reviewed the [**Contributor Guidelines**](https://github.com/glotzerlab/freud/blob/main/CONTRIBUTING.md).
- [ ] I agree with the terms of the [**Freud Contributor Agreement**](https://github.com/glotzerlab/freud/blob/main/ContributorAgreement.md).
- [ ] My name is on the [list of contributors](https://github.com/glotzerlab/freud/blob/main/doc/source/reference/credits.rst) in the pull request source branch.
- [ ] I have updated the [Change log](https://github.com/glotzerlab/freud/blob/main/ChangeLog.md).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6cdad3fe6249`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.