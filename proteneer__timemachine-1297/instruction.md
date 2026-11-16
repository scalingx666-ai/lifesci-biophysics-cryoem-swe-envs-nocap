# Issue

**Title:** Improves performance of Local MD potentials

## Description

* Believe this was originally due to flaky tests, but was resolved with 
* Removes the `pow` calls in the flat bottom kernel. Makes for a 2x speed up of the kernel, though doesn't appear to matter much for the benchmarks.
* Nightlies all pass


## Kernel Timings

Kernels are 2x faster with the removal of the `pow` calls, both in float/double.

Ran `pytest -k flat_bottom_bond` to generate the timings

### Master
```
CUDA Kernel Statistics:

 Time (%)  Total Time (ns)  Instances  Avg (ns)  Med (ns)  Min (ns)  Max (ns)  StdDev (ns)                                                  Name                                                
 --------  ---------------  ---------  --------  --------  --------  --------  -----------  ----------------------------------------------------------------------------------------------------
     24.0          385,152         20  19,257.6  19,552.5    10,400    25,185      4,918.7  void timemachine::k_log_flat_bottom_bond<double>(int, const double *, const double *, const double …
     23.1          370,527         20  18,526.4  18,959.5    10,080    25,119      4,813.2  void timemachine::k_log_flat_bottom_bond<float>(int, const double *, const double *, const double *…
     22.4          359,233         20  17,961.7  18,096.0     4,288    28,096      7,806.7  void timemachine::k_flat_bottom_bond<double>(int, const double *, const double *, const double *, c…
     20.7          332,351         20  16,617.6  17,088.0     3,680    25,376      7,374.3  void timemachine::k_flat_bottom_bond<float>(int, const double *, const double *, const double *, co…
      9.6          154,459         48   3,217.9   3,168.0     3,135     3,776        130.7  void timemachine::k_accumulate_energy<(unsigned int)512>(int, const __int128 *, __int128 *)      
```

### PR
```
CUDA Kernel Statistics:

 Time (%)  Total Time (ns)  Instances  Avg (ns)  Med (ns)  Min (ns)  Max (ns)  StdDev (ns)                                                  Name                                                
 --------  ---------------  ---------  --------  --------  --------  --------  -----------  ----------------------------------------------------------------------------------------------------
     26.9          178,465         20   8,923.3   9,824.0     5,472    11,264      2,239.2  void timemachine::k_log_flat_bottom_bond<double>(int, const double *, const double *, const double …
     23.3          154,113         48   3,210.7   3,137.0     3,135     3,776        149.7  void timemachine::k_accumulate_energy<(unsigned int)512>(int, const __int128 *, __int128 *)         
     22.4          148,032         20   7,401.6   8,864.0     4,608     9,472      2,120.3  void timemachine::k_log_flat_bottom_bond<float>(int, const double *, const double *, const double *…
     15.1          100,256         20   5,012.8   5,056.0     4,256     6,368        582.1  void timemachine::k_flat_bottom_bond<double>(int, const double *, const double *, const double *, c…
     12.3           81,345         20   4,067.3   4,064.0     3,744     4,512        216.0  void timemachine::k_flat_bottom_bond<float>(int, const double *, const double *, const double *, co…
```

## Benchmarks
Only showing the local MD benchmarks as these changes only impacts local MD performance. Seems to be pretty much a wash when run in local MD, which appears to be due to the fact that this was never a bottleneck for local MD.

It appears that the local MD benchmarks are not converged. Looking at the output of `nsys` shows that the total run time of the flat bottom restraint went from 2.9% to 1.4% of the total runtime. Think that the other kernels (and perhaps set up time) is where the overhead is.

Run on A10, Cuda Arch 8.6

### Master
```
hif2a-rbfe-local: N=8840 speed: 1335.10ns/day dt: 2.5fs (ran 100000 steps in 16.18s)
solvent-rbfe-local: N=6317 speed: 1468.79ns/day dt: 2.5fs (ran 100000 steps in 14.71s)
```

### PR
```
hif2a-rbfe-local: N=8840 speed: 1331.59ns/day dt: 2.5fs (ran 100000 steps in 16.23s)
solvent-rbfe-local: N=6317 speed: 1435.01ns/day dt: 2.5fs (ran 100000 steps in 15.06s)
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6e188c541ad4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.