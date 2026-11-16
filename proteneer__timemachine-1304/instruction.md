# Issue

**Title:** Improve memory throughput of atom by atom

## Description

* ~10% speed up of the kernel at batch sizes 200-300
* This is thanks to an improvement of the memory throughput as measured by Nsight-compute. Tried also doing warp swaps (each y block load WARP_SIZE atoms) in the y block, but that actually reduced the performance of the kernel to my surprise.

# Benchmarks
There doesn't appear to be a significant improvement to the ns/day benchmarks, but do see a ~10% speed up in the water sampling example.

## NS/Day Benchmarks
Only looking at the hif2a case, since it is the only benchmark with water sampling
### Master
```
hif2a-rbfe-barostat-interval-25: N=8840 speed: 783.71ns/day dt: 2.5fs (ran 100000 steps in 27.56s)
hif2a-rbfe-barostat-interval-25-water-sampling-interval-400: N=8840 speed: 726.65ns/day dt: 2.5fs (ran 100000 steps in 29.73s)
```

### PR
```
hif2a-rbfe-barostat-interval-25: N=8840 speed: 781.30ns/day dt: 2.5fs (ran 100000 steps in 27.65s)
hif2a-rbfe-barostat-interval-25-water-sampling-interval-400: N=8840 speed: 732.96ns/day dt: 2.5fs (ran 100000 steps in 29.47s)
```

## Water Sampling Example

Result of running `python -u examples/water_sampling_mc.py --water_pdb timemachine/datasets/water_exchange/bb_6_waters.pdb --ligand_sdf timemachine/datasets/water_exchange/bb_centered_espaloma.sdf --out_cif traj_6_waters.cif --md_steps_per_batch 10000 --mc_steps_per_batch 10000 --insertion_type targeted --use_hmr 1 --batch_size 250`

Time per mc move is reduced by ~9%

### Master
```
2 / 10000 | density 991.6458074986836 | # of waters in spherical region 4 | md step: 0 | time per mc move: 4109.1798ns
4 / 20000 | density 990.2169202722931 | # of waters in spherical region 2 | md step: 10000 | time per mc move: 4040.6117ns
4 / 30000 | density 992.2573134334189 | # of waters in spherical region 2 | md step: 20000 | time per mc move: 3864.6266ns
4 / 40000 | density 987.5691282363993 | # of waters in spherical region 2 | md step: 30000 | time per mc move: 3865.3837ns
4 / 50000 | density 994.4733590379498 | # of waters in spherical region 2 | md step: 40000 | time per mc move: 3860.3977ns
```

### PR
```
2 / 10000 | density 991.6458074986836 | # of waters in spherical region 4 | md step: 0 | time per mc move: 3741.9775ns
4 / 20000 | density 990.2169202722931 | # of waters in spherical region 2 | md step: 10000 | time per mc move: 3675.4506ns
4 / 30000 | density 992.2573134334189 | # of waters in spherical region 2 | md step: 20000 | time per mc move: 3516.9444ns
4 / 40000 | density 987.5691282363993 | # of waters in spherical region 2 | md step: 30000 | time per mc move: 3515.0994ns
4 / 50000 | density 994.4733590379498 | # of waters in spherical region 2 | md step: 40000 | time per mc move: 3511.0445ns
```

## Kernel Timings

Looking at the output of the following command
`nsys profile --stats=true -t nvtx,cuda python -u examples/water_sampling_mc.py --water_pdb timemachine/datasets/water_exchange/bb_6_waters.pdb --ligand_sdf timemachine/datasets/water_exchange/bb_centered_espaloma.sdf --out_cif traj_6_waters.cif --iterations 1 --equilibration_steps 0 --md_steps_per_batch 0 --mc_steps_per_batch 10000 --insertion_type targeted --use_hmr 1 --batch_size 250`

`k_atom_by_atom_energies` went from ~250k ns to ~210k ns by improving the memory throughput.

### Master
```
CUDA Kernel Statistics:

 Time (%)  Total Time (ns)  Instances  Avg (ns)   Med (ns)   Min (ns)  Max (ns)  StdDev (ns)                                                  Name                                                
 --------  ---------------  ---------  ---------  ---------  --------  --------  -----------  ----------------------------------------------------------------------------------------------------
     53.4         25213920         98   257284.9   257227.5    254971    258428        470.6  void timemachine::k_atom_by_atom_energies<float>(int, int, const int *, const double *, const doubl…
      8.0          3761820         49    76771.8    76735.0     76126     78143        373.1  void timemachine::k_adjust_energies<float, (bool)1>(int, int, int, int, const int *, const int *, c…
      8.0          3760003         49    76734.8    76703.0     76223     77598        303.1  void timemachine::k_adjust_energies<float, (bool)0>(int, int, int, int, const int *, const int *, c…
      7.0          3284267         49    67025.9    67007.0     66751     67327        154.6  void timemachine::k_set_sampled_energy_block<float, (int)512>(int, int, int, int, const int *, cons…
      4.5          2127321         49    43414.7    41151.0     40927     54047       4835.1  timemachine::k_store_exchange_move(int, int, const int *, const int *, const int *, const int *, co…
      4.1          1917985          1  1917985.0  1917985.0   1917985   1917985          0.0  void timemachine::k_compute_nonbonded_target_atom_energies<float, (int)64>(int, int, const int *, c…
      2.4          1128244         99    11396.4     3744.0      3104     20352       8225.1  void timemachine::k_convert_energies_to_log_weights<float>(int, T1, const __int128 *, T1 *)         
      1.2           575702          4   143925.5   128669.5    128542    189821      30597.2  void generate_seed_pseudo<rng_config<curandStateXORWOW, (curandOrdering)101>>(unsigned long long, u…
      1.2           553045         98     5643.3     5600.0      5535      6592        124.4  void timemachine::k_segmented_exp_sub_max<float>(int, const int *, const T1 *, const T1 *, T1 *)    
      0.9           414041         49     8449.8     8448.0      7584      8896        226.0  void timemachine::k_setup_destination_weights_for_targeted<float>(int, int, const int *, const int …
<truncated>
```

### PR
```
CUDA Kernel Statistics:

 Time (%)  Total Time (ns)  Instances  Avg (ns)   Med (ns)   Min (ns)  Max (ns)  StdDev (ns)                                                  Name                                                
 --------  ---------------  ---------  ---------  ---------  --------  --------  -----------  ----------------------------------------------------------------------------------------------------
     49.0         21030252         98   214594.4   214413.0    212252    216733        742.2  void timemachine::k_atom_by_atom_energies<float>(int, int, const int *, const double *, const doubl…
      8.7          3717464         49    75866.6    75839.0     75422     76415        266.9  void timemachine::k_adjust_energies<float, (bool)0>(int, int, int, int, const int *, const int *, c…
      8.7          3716444         49    75845.8    75871.0     75358     76479        251.8  void timemachine::k_adjust_energies<float, (bool)1>(int, int, int, int, const int *, const int *, c…
      7.7          3290381         49    67150.6    67135.0     66911     67423        131.8  void timemachine::k_set_sampled_energy_block<float, (int)512>(int, int, int, int, const int *, cons…
      5.0          2130176         49    43473.0    41343.0     40799     53920       4859.2  timemachine::k_store_exchange_move(int, int, const int *, const int *, const int *, const int *, co…
      4.5          1919904          1  1919904.0  1919904.0   1919904   1919904          0.0  void timemachine::k_compute_nonbonded_target_atom_energies<float, (int)64>(int, int, const int *, c…
      2.6          1136754         99    11482.4     3552.0      3136     20287       8290.9  void timemachine::k_convert_energies_to_log_weights<float>(int, T1, const __int128 *, T1 *)         
      1.3           575959          4   143989.8   128606.0    128190    190557      31045.7  void generate_seed_pseudo<rng_config<curandStateXORWOW, (curandOrdering)101>>(unsigned long long, u…
      1.3           555304         98     5666.4     5616.0      5535      6688        172.3  void timemachine::k_segmented_exp_sub_max<float>(int, const int *, const T1 *, const T1 *, T1 *)    
      1.0           417783         49     8526.2     8544.0      7744      9440        248.1  void timemachine::k_setup_destination_weights_for_targeted<float>(int, int, const int *, const int …
      1.0           411951         98     4203.6     4239.5      3360      5153        226.4  void cub::DeviceSegmentedReduceKernel<cub::DeviceReducePolicy<float, float, int, cub::Max>::Policy6…
      0.9           386291         98     3941.7     3904.0      2944      4512        169.8  void cub::DeviceSegmentedReduceKernel<cub::DeviceReducePolicy<float, float, int, cub::Sum>::Policy6…
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9427c1df0350`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.