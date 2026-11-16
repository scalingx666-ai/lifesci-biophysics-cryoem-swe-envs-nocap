# Issue

**Title:** Add lambda schedule tuned to Bisection

## Description

For vacuum it makes very little different to minimize more states than required (2 * (num_windows - 2)), but in solvent/complex it can get expensive. Seen in 30k atom systems that minimizing (local minimization) a new window takes 20-30 seconds which is ~15 minutes or so (for num windows = 48) spent minimizing states that aren't actually used.

| Master | PR |
|--------|--------|
| ![vacuum_100_benchmarks](https://github.com/user-attachments/assets/1c67408f-cf96-47ec-a105-45d331c661e8) | ![vacuum_100_benchmarks_bisection](https://github.com/user-attachments/assets/db68147d-0915-4c92-b1db-49c519886972) |
| ![solvent_100_benchmarks](https://github.com/user-attachments/assets/ed89b392-308a-4f80-b508-12238aac7c94) | ![solvent_100_benchmarks_bisection](https://github.com/user-attachments/assets/089a0b56-6b0b-4f18-9382-c152b24c65e1) |
| ![complex_100_benchmarks](https://github.com/user-attachments/assets/41a1b7c4-8b68-4c69-9335-b5ad3c3adc64) | ![complex_100_benchmarks_bisection](https://github.com/user-attachments/assets/dc60fa5a-06de-4584-865c-e1f083687284) |

The performance improvement with this change is about 10% (vacuum) to 20% (solvent/complex). Though it is worth noting that **the performance improvement is related to the number of windows needed and what the maximum number of windows are**. The performance gain will be O(1) and **not scale with the number of frames**, the benchmarks are done here with 100 frames to match the bisection stage of HREX/Bisection. 

Note that the bisection timings after this change are noisier since there are still new initial states that have to be minimized depending on schedule generated bisection. This should be strictly faster than the previous implementation (where num windows are even).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5ea753f15b97`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.