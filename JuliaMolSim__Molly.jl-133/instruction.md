# Issue

**Title:** Non-atomic pairwise force summation kernels.

## Description

# Benchmark results before any changes
# Job Properties
* Time of benchmark: 7 Jun 2023 - 19:30
* Package commit: cc575d
* Julia commit: 8e6305
* Julia command flags: None
* Environment variables: `JULIA_NUM_THREADS => 16`

## Results
Below is a table of this job's results, obtained by running the benchmarks.
The values listed in the `ID` column have the structure `[parent_group, child_group, ..., key]`, and can be used to
index into the BaseBenchmarks suite to retrieve the corresponding benchmarks.
The percentages accompanying time and memory values in the below table are noise tolerances. The "true"
time/memory value for a given benchmark is expected to fall within this percentage of the reported value.
An empty cell means that the value was zero.

| ID                                        | time            | GC time   | memory          | allocations |
|-------------------------------------------|----------------:|----------:|----------------:|------------:|
| `["interactions", "Coulomb energy"]`      |   8.585 ns (5%) |           |                 |             |
| `["interactions", "Coulomb force"]`       |  10.210 ns (5%) |           |                 |             |
| `["interactions", "HarmonicBond energy"]` |  13.589 ns (5%) |           |                 |             |
| `["interactions", "HarmonicBond force"]`  |  20.126 ns (5%) |           |                 |             |
| `["interactions", "LennardJones energy"]` |  14.764 ns (5%) |           |                 |             |
| `["interactions", "LennardJones force"]`  |  16.534 ns (5%) |           |                 |             |
| `["protein", "CPU parallel NL"]`          |    2.109 s (5%) | 26.741 ms | 504.39 MiB (1%) |        7430 |
| `["simulation", "CPU NL"]`                | 136.302 ms (5%) |           |  34.51 MiB (1%) |       11749 |
| `["simulation", "CPU f32 NL"]`            | 131.337 ms (5%) |           |  22.23 MiB (1%) |       11748 |
| `["simulation", "CPU f32"]`               | 723.609 ms (5%) |           |  18.42 MiB (1%) |       11057 |
| `["simulation", "CPU parallel NL"]`       | 148.578 ms (5%) |           |  99.92 MiB (1%) |       41661 |
| `["simulation", "CPU parallel f32 NL"]`   |  87.299 ms (5%) |           |  59.08 MiB (1%) |       41654 |
| `["simulation", "CPU parallel f32"]`      | 150.689 ms (5%) |           |  49.74 MiB (1%) |       36257 |
| `["simulation", "CPU parallel"]`          | 171.384 ms (5%) |           |  90.58 MiB (1%) |       36267 |
| `["simulation", "CPU"]`                   | 655.085 ms (5%) |           |  30.69 MiB (1%) |       11058 |
| `["simulation", "GPU NL"]`                |  93.107 ms (5%) |           |  13.66 MiB (1%) |       86877 |
| `["simulation", "GPU f32 NL"]`            |  87.481 ms (5%) |           |  10.43 MiB (1%) |       81447 |
| `["simulation", "GPU f32"]`               |  90.161 ms (5%) |           |   9.78 MiB (1%) |       70666 |
| `["simulation", "GPU"]`                   |  93.585 ms (5%) |           |  12.99 MiB (1%) |       76096 |
| `["spatial", "vector"]`                   |  11.215 ns (5%) |           |                 |             |
| `["spatial", "vector_1D"]`                |   8.266 ns (5%) |           |                 |             |

## Benchmark Group List
Here's a list of all the benchmark groups executed by this job:

- `["interactions"]`
- `["protein"]`
- `["simulation"]`
- `["spatial"]`

## Julia versioninfo
```
Julia Version 1.9.0
Commit 8e630552924 (2023-05-07 11:25 UTC)
Platform Info:
  OS: Linux (x86_64-linux-gnu)
      Ubuntu 20.04.6 LTS
  uname: Linux 5.4.0-146-generic #163-Ubuntu SMP Fri Mar 17 18:26:02 UTC 2023 x86_64 x86_64
  CPU: Intel(R) Xeon(R) CPU E5-2603 v4 @ 1.70GHz: 
                 speed         user         nice          sys         idle          irq
       #1-12  1200 MHz   41107039 s     172497 s    5408181 s  518383963 s          0 s
  Memory: 377.7989311218262 GB (363221.26953125 MB free)
  Uptime: 4.72816548e6 sec
  Load Avg:  4.25  3.26  2.61
  WORD_SIZE: 64
  LIBM: libopenlibm
  LLVM: libLLVM-14.0.6 (ORCJIT, broadwell)
  Threads: 16 on 12 virtual cores
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `17f20ed3abd0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.