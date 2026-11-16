# Issue

**Title:** Improve performance of Vacuum Related Kernels

## Description

* Speeds up NonbondedPrecomputed by 3x
* Removes a MemsetAsync call
* Turns kernels into while loops, which allow us to play with reducing the total number of blocks and improving memory throughput by having a single block do multiple blocks of work. 


## Kernel Profiling

* Reduces MemsetAsync from 3.4% of run time to 2.1% by pushing zeroing of forces to `k_update_forward_baoab`
* Speeds up nonbonded precomputed 3x (12k -> 4k ns)
* ~1% speed up to Harmonic bond, probably pretty close to a wash.

### Master
```
CUDA API Statistics:

 Time (%)  Total Time (ns)  Num Calls  Avg (ns)   Med (ns)   Min (ns)   Max (ns)    StdDev (ns)            Name           
 --------  ---------------  ---------  ---------  ---------  --------  -----------  -----------  -------------------------
     52.5      199,987,588      1,341  149,133.2   13,200.0       139  181,733,155  4,962,363.0  cudaMalloc               
     21.1       80,534,569     14,588    5,520.6    3,805.0     3,308       57,489      4,240.8  cudaLaunchKernel         
      6.3       24,180,522        892   27,108.2   24,298.0     4,323       93,882     11,936.6  cudaMemcpy               
      5.9       22,407,838      1,345   16,660.1    9,945.0       334   10,629,381    289,749.3  cudaFree                 
      4.9       18,808,038     22,170      848.4      612.0       482       17,036        753.8  cudaEventRecord          
      4.9       18,792,276     22,170      847.6      636.0       485       16,739        730.1  cudaStreamWaitEvent      
      3.4       13,088,407      2,651    4,937.2    2,756.0     2,437       30,162      4,723.1  cudaMemsetAsync          
<truncated>

CUDA Kernel Statistics:

 Time (%)  Total Time (ns)  Instances  Avg (ns)   Med (ns)   Min (ns)  Max (ns)  StdDev (ns)                                                  Name                                                
 --------  ---------------  ---------  ---------  ---------  --------  --------  -----------  ----------------------------------------------------------------------------------------------------
     38.0       27,388,442      2,217   12,353.8   11,968.0    11,008    17,472      1,439.9  void timemachine::k_nonbonded_precomputed<float>(int, const double *, const double *, const double …
     15.5       11,201,532      2,217    5,052.6    5,056.0     4,160     7,808        236.5  void timemachine::k_periodic_torsion<float, (int)3>(int, const double *, const double *, const int …
     12.1        8,764,182      2,217    3,953.2    3,871.0     3,072     7,008        329.7  void timemachine::k_harmonic_angle_stable<float>(int, const double *, const double *, const int *, …
     11.2        8,097,344      2,217    3,652.4    3,616.0     2,816    12,992        292.3  void timemachine::k_chiral_atom_restraint<float>(int, const double *, const double *, const int *, …
      8.8        6,371,956      2,217    2,874.1    2,784.0     2,528    13,120        349.5  void timemachine::k_harmonic_bond<float>(int, const double *, const double *, const int *, unsigned…
      7.6        5,502,909      2,000    2,751.5    2,751.0     2,687    10,592        191.0  void timemachine::k_update_forward_baoab<float, (int)3>(int, T1, const unsigned int *, const T1 *, …
      5.8        4,184,478      1,302    3,213.9    3,168.0     2,432     5,088        150.5  void timemachine::k_accumulate_energy<(unsigned int)512>(int, const __int128 *, __int128 *)         
      0.7          484,067        200    2,420.3    2,368.0     2,336     2,913        109.2  void gen_sequenced<curandStateXORWOW, float2, normal_args_st, &curand_normal_scaled2<curandStateXOR…
      0.2          155,584          1  155,584.0  155,584.0   155,584   155,584          0.0  void generate_seed_pseudo<rng_config<curandStateXORWOW, (curandOrdering)101>>(unsigned long long, u…
```

### PR
```
CUDA API Statistics:

 Time (%)  Total Time (ns)  Num Calls  Avg (ns)   Med (ns)   Min (ns)   Max (ns)    StdDev (ns)            Name           
 --------  ---------------  ---------  ---------  ---------  --------  -----------  -----------  -------------------------
     51.3      203,625,261      1,479  137,677.7   12,972.0       131  184,613,480  4,800,100.6  cudaMalloc               
     22.8       90,605,484     14,841    6,105.1    4,241.0     3,561       58,602      4,459.8  cudaLaunchKernel         
      6.6       26,117,367        984   26,542.0   23,525.5     4,713      321,597     14,918.2  cudaMemcpy               
      6.0       23,941,801      1,483   16,144.2    9,766.0       332   11,230,940    291,569.8  cudaFree                 
      5.3       21,183,519     22,400      945.7      685.0       516       17,288        871.5  cudaEventRecord          
      5.3       20,912,185     22,400      933.6      700.0       479       17,131        863.1  cudaStreamWaitEvent      
      2.1        8,370,434        720   11,625.6    8,810.0     2,212       33,439      5,235.5  cudaMemsetAsync          
<truncated>

CUDA Kernel Statistics:

 Time (%)  Total Time (ns)  Instances  Avg (ns)   Med (ns)   Min (ns)  Max (ns)  StdDev (ns)                                                  Name                                                
 --------  ---------------  ---------  ---------  ---------  --------  --------  -----------  ----------------------------------------------------------------------------------------------------
     20.6       11,359,197      2,240    5,071.1    5,088.0     4,192     7,488        236.6  void timemachine::k_periodic_torsion<float, (int)3>(int, const double *, const double *, const int …
     17.1        9,430,006      2,240    4,209.8    4,096.0     3,264    12,448        403.7  void timemachine::k_nonbonded_precomputed<float>(int, const double *, const double *, const double …
     16.2        8,931,372      2,240    3,987.2    3,904.0     3,072    12,447        327.4  void timemachine::k_harmonic_angle_stable<float>(int, const double *, const double *, const int *, …
     14.8        8,156,151      2,240    3,641.1    3,616.0     2,720     7,392        191.7  void timemachine::k_chiral_atom_restraint<float>(int, const double *, const double *, const int *, …
     11.5        6,319,033      2,240    2,821.0    2,752.0     2,208    12,320        348.4  void timemachine::k_harmonic_bond<float>(int, const double *, const double *, const int *, unsigned…
     10.1        5,544,387      2,000    2,772.2    2,752.0     2,719    11,295        260.8  void timemachine::k_update_forward_baoab<float, (int)3>(int, T1, const unsigned int *, const T1 *, …
      8.4        4,627,738      1,440    3,213.7    3,168.0     2,432     4,992        163.3  void timemachine::k_accumulate_energy<(unsigned int)512>(int, const __int128 *, __int128 *)         
      0.9          486,079        200    2,430.4    2,368.0     2,336     2,977        119.0  void gen_sequenced<curandStateXORWOW, float2, normal_args_st, &curand_normal_scaled2<curandStateXOR…
      0.3          156,287          1  156,287.0  156,287.0   156,287   156,287          0.0  void generate_seed_pseudo<rng_config<curandStateXORWOW, (curandOrdering)101>>(unsigned long long, u…
```

## Benchmarks
A bit faster overall, but the significant improvement comes for `vacuum` where it is 10% faster. On my laptops' RTX 2070, this went from 10,000ns/day to 17,000ns/day, so seems to have a pretty dramatic effect depending on GPU.

A10 Cuda Arch 8.6

### Master
```
dhfr-apo: N=23558 speed: 742.98ns/day dt: 2.5fs (ran 100000 steps in 29.08s)
dhfr-apo-barostat-interval-25: N=23558 speed: 664.13ns/day dt: 2.5fs (ran 100000 steps in 32.53s)

hif2a-apo: N=8805 speed: 1292.51ns/day dt: 2.5fs (ran 100000 steps in 16.71s)
hif2a-apo-barostat-interval-25: N=8805 speed: 1075.76ns/day dt: 2.5fs (ran 100000 steps in 20.08s)
hif2a-rbfe-barostat-interval-25: N=8840 speed: 825.24ns/day dt: 2.5fs (ran 100000 steps in 26.18s)
hif2a-rbfe-local: N=8840 speed: 1341.98ns/day dt: 2.5fs (ran 100000 steps in 16.10s)
hif2a-rbfe-barostat-interval-25-water-sampling-interval-400: N=8840 speed: 764.77ns/day dt: 2.5fs (ran 100000 steps in 28.25s)

solvent-apo: N=6282 speed: 1543.61ns/day dt: 2.5fs (ran 100000 steps in 14.00s)
solvent-apo-barostat-interval-25: N=6282 speed: 1364.97ns/day dt: 2.5fs (ran 100000 steps in 15.83s)
solvent-rbfe-barostat-interval-25: N=6317 speed: 1132.15ns/day dt: 2.5fs (ran 100000 steps in 19.08s)
solvent-rbfe-local: N=6317 speed: 1458.70ns/day dt: 2.5fs (ran 100000 steps in 14.81s)

vacuum-rbfe: N=35 speed: 8926.69ns/day dt: 2.5fs (ran 100000 steps in 2.42s)

NonbondedInteractionGroup_f32: N=8840 Frames=1000 Params=5 speed: 1205.09 executions/seconds (ran 10000 potentials in 8.30s) du_dp=True, du_dx=True, u=True
NonbondedInteractionGroup_f64: N=8840 Frames=1000 Params=5 speed: 627.69 executions/seconds (ran 10000 potentials in 15.93s) du_dp=True, du_dx=True, u=True
HarmonicBond_f32: N=8840 Frames=1000 Params=5 speed: 1544.32 executions/seconds (ran 10000 potentials in 6.48s) du_dp=True, du_dx=True, u=True
HarmonicBond_f64: N=8840 Frames=1000 Params=5 speed: 1542.78 executions/seconds (ran 10000 potentials in 6.48s) du_dp=True, du_dx=True, u=True
HarmonicAngleStable_f32: N=8840 Frames=1000 Params=5 speed: 1498.51 executions/seconds (ran 10000 potentials in 6.67s) du_dp=True, du_dx=True, u=True
HarmonicAngleStable_f64: N=8840 Frames=1000 Params=5 speed: 1475.10 executions/seconds (ran 10000 potentials in 6.78s) du_dp=True, du_dx=True, u=True
PeriodicTorsion_f32: N=8840 Frames=1000 Params=5 speed: 1504.27 executions/seconds (ran 10000 potentials in 6.65s) du_dp=True, du_dx=True, u=True
PeriodicTorsion_f64: N=8840 Frames=1000 Params=5 speed: 1476.62 executions/seconds (ran 10000 potentials in 6.77s) du_dp=True, du_dx=True, u=True
ChiralAtomRestraint_f32: N=8840 Frames=1000 Params=5 speed: 1723.02 executions/seconds (ran 10000 potentials in 5.80s) du_dp=True, du_dx=True, u=True
ChiralAtomRestraint_f64: N=8840 Frames=1000 Params=5 speed: 1700.76 executions/seconds (ran 10000 potentials in 5.88s) du_dp=True, du_dx=True, u=True
NonbondedPairListPrecomputed_f32: N=8840 Frames=1000 Params=5 speed: 1650.76 executions/seconds (ran 10000 potentials in 6.06s) du_dp=True, du_dx=True, u=True
NonbondedPairListPrecomputed_f64: N=8840 Frames=1000 Params=5 speed: 1627.50 executions/seconds (ran 10000 potentials in 6.14s) du_dp=True, du_dx=True, u=True
Nonbonded_f32: N=8840 Frames=1000 Params=5 speed: 944.29 executions/seconds (ran 10000 potentials in 10.59s) du_dp=True, du_dx=True, u=True
Nonbonded_f64: N=8840 Frames=1000 Params=5 speed: 131.67 executions/seconds (ran 10000 potentials in 75.95s) du_dp=True, du_dx=True, u=True
SummedPotential(NonbondedInteractionGroup, NonbondedInteractionGroup)_f32: N=8840 Frames=1000 Params=5 speed: 944.57 executions/seconds (ran 10000 potentials in 10.59s) du_dp=True, du_dx=True, u=True
SummedPotential(NonbondedInteractionGroup, NonbondedInteractionGroup)_f64: N=8840 Frames=1000 Params=5 speed: 474.32 executions/seconds (ran 10000 potentials in 21.08s) du_dp=True, du_dx=True, u=True
```

#### HREX
![vacuum_100_benchmarks_master](https://github.com/proteneer/timemachine/assets/5840832/e7fa130d-fd6d-4633-a67c-65c84474537c)



### PR
```
dhfr-apo: N=23558 speed: 750.36ns/day dt: 2.5fs (ran 100000 steps in 28.79s)
dhfr-apo-barostat-interval-25: N=23558 speed: 675.82ns/day dt: 2.5fs (ran 100000 steps in 31.96s)

hif2a-apo: N=8805 speed: 1303.73ns/day dt: 2.5fs (ran 100000 steps in 16.57s)
hif2a-apo-barostat-interval-25: N=8805 speed: 1085.13ns/day dt: 2.5fs (ran 100000 steps in 19.91s)
hif2a-rbfe-barostat-interval-25: N=8840 speed: 833.15ns/day dt: 2.5fs (ran 100000 steps in 25.93s)
hif2a-rbfe-local: N=8840 speed: 1411.61ns/day dt: 2.5fs (ran 100000 steps in 15.31s)
hif2a-rbfe-barostat-interval-25-water-sampling-interval-400: N=8840 speed: 774.18ns/day dt: 2.5fs (ran 100000 steps in 27.90s)

solvent-apo: N=6282 speed: 1574.22ns/day dt: 2.5fs (ran 100000 steps in 13.72s)
solvent-apo-barostat-interval-25: N=6282 speed: 1423.94ns/day dt: 2.5fs (ran 100000 steps in 15.17s)
solvent-rbfe-barostat-interval-25: N=6317 speed: 1150.62ns/day dt: 2.5fs (ran 100000 steps in 18.78s)
solvent-rbfe-local: N=6317 speed: 1502.11ns/day dt: 2.5fs (ran 100000 steps in 14.38s)

vacuum-rbfe: N=35 speed: 9942.95ns/day dt: 2.5fs (ran 100000 steps in 2.17s)

NonbondedInteractionGroup_f32: N=8840 Frames=1000 Params=5 speed: 1158.42 executions/seconds (ran 10000 potentials in 8.63s) du_dp=True, du_dx=True, u=True
NonbondedInteractionGroup_f64: N=8840 Frames=1000 Params=5 speed: 618.59 executions/seconds (ran 10000 potentials in 16.17s) du_dp=True, du_dx=True, u=True
HarmonicBond_f32: N=8840 Frames=1000 Params=5 speed: 1443.89 executions/seconds (ran 10000 potentials in 6.93s) du_dp=True, du_dx=True, u=True
HarmonicBond_f64: N=8840 Frames=1000 Params=5 speed: 1457.79 executions/seconds (ran 10000 potentials in 6.86s) du_dp=True, du_dx=True, u=True
HarmonicAngleStable_f32: N=8840 Frames=1000 Params=5 speed: 1411.12 executions/seconds (ran 10000 potentials in 7.09s) du_dp=True, du_dx=True, u=True
HarmonicAngleStable_f64: N=8840 Frames=1000 Params=5 speed: 1388.36 executions/seconds (ran 10000 potentials in 7.20s) du_dp=True, du_dx=True, u=True
PeriodicTorsion_f32: N=8840 Frames=1000 Params=5 speed: 1430.05 executions/seconds (ran 10000 potentials in 6.99s) du_dp=True, du_dx=True, u=True
PeriodicTorsion_f64: N=8840 Frames=1000 Params=5 speed: 1391.95 executions/seconds (ran 10000 potentials in 7.18s) du_dp=True, du_dx=True, u=True
ChiralAtomRestraint_f32: N=8840 Frames=1000 Params=5 speed: 1621.08 executions/seconds (ran 10000 potentials in 6.17s) du_dp=True, du_dx=True, u=True
ChiralAtomRestraint_f64: N=8840 Frames=1000 Params=5 speed: 1609.86 executions/seconds (ran 10000 potentials in 6.21s) du_dp=True, du_dx=True, u=True
NonbondedPairListPrecomputed_f32: N=8840 Frames=1000 Params=5 speed: 1623.02 executions/seconds (ran 10000 potentials in 6.16s) du_dp=True, du_dx=True, u=True
NonbondedPairListPrecomputed_f64: N=8840 Frames=1000 Params=5 speed: 1559.34 executions/seconds (ran 10000 potentials in 6.41s) du_dp=True, du_dx=True, u=True
Nonbonded_f32: N=8840 Frames=1000 Params=5 speed: 908.85 executions/seconds (ran 10000 potentials in 11.00s) du_dp=True, du_dx=True, u=True
Nonbonded_f64: N=8840 Frames=1000 Params=5 speed: 130.56 executions/seconds (ran 10000 potentials in 76.59s) du_dp=True, du_dx=True, u=True
SummedPotential(NonbondedInteractionGroup, NonbondedInteractionGroup)_f32: N=8840 Frames=1000 Params=5 speed: 900.08 executions/seconds (ran 10000 potentials in 11.11s) du_dp=True, du_dx=True, u=True
SummedPotential(NonbondedInteractionGroup, NonbondedInteractionGroup)_f64: N=8840 Frames=1000 Params=5 speed: 465.91 executions/seconds (ran 10000 potentials in 21.46s) du_dp=True, du_dx=True, u=True

```

#### HREX
About 200ns faster in HREX with 48 windows, about 5%

![vacuum_100_benchmarks](https://github.com/proteneer/timemachine/assets/5840832/1519aed0-3da0-4841-bf53-c625d37f3f10)


## Todos

- [x] Update the determinism tests, after #1290 is merged

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `df9419aab1b4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.