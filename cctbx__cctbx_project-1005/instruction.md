# Issue

**Title:** New cctbx.xfel.merge worker for computing and filtering on ΔCC½

## Description

This is a means of filtering out lattices that degrade the overall CC½. Uses the σ-τ method from [Assmann 2016](https://journals.iucr.org/j/issues/2016/03/00/zw5005/#FD4) to avoid splitting the data into odd/even datasets. Enable by adding the deltaccint worker after the group worker.

Includes parameter iqr_ratio = 10. If the ΔCC½ filter is enabled, first compute CC½ when removing every image one at a time, then compute the IQR of these CC½s.  Remove all lattices whose contribution degrades CC½ by more than IQR * iqr_ratio above the median. You can discover a good IQR by running the program once, examining the log file where possible values are listed, and running it again with the best value.

Implementation heavily uses MPI for fast computation of ΔCC½ on thousands of images.  Current run time with this version is ~5 minutes for 8.8k images.

Example output in the log for a PSII dataset:

```
STEP 11: ΔCC½ statistics (σ-τ method)
Beginning ΔCC½ analysis (σ-τ method from Assmann 2016)
Removing reflections with less than 3 measurements
N experiments after filtering: 8816

Five number summary
 91.3091% min
 91.3109% q1
 91.3111% median
 91.3114% q3
 91.4728% max

Showing ΔCC½ of the worst 30 lattices and IQR ratio needed to remove them
 ΔCC½ (%) IQR ratio Lattices removed
 91.3172       11.7 30
 91.3174       12.1 29
 91.3175       12.3 28
 91.3175       12.4 27
 91.3178       12.9 26
 91.3179       13.1 25
 91.3182       13.7 24
 91.3183       13.8 23
 91.3184       14.1 22
 91.3185       14.2 21
 91.3188       14.8 20
 91.3192       15.5 19
 91.3199       16.9 18
 91.3200       17.1 17
 91.3215       20.1 16
 91.3218       20.5 15
 91.3219       20.7 14
 91.3222       21.3 13
 91.3350       45.9 12
 91.3403       56.1 11
 91.3403       56.2 10
 91.3404       56.3 9
 91.3407       56.9 8
 91.3408       57.1 7
 91.3409       57.3 6
 91.3410       57.6 5
 91.3413       58.1 4
 91.3414       58.2 3
 91.3649      103.5 2
 91.4728      311.1 1

Removing 29 experiments out of 8816 using IQR ratio 12.0
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `57cd1f9e0009`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.