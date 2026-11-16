# Issue

**Title:** Raise exception on unstable simulations

## Description

Currently when a simulation blows up if the coordinates reach a certain magnitude an invalid memory access can take place due to floating point precision in https://github.com/proteneer/timemachine/blob/80e4aae47169688200a802612c48cc54a0ee1010/timemachine/cpp/src/kernels/k_hilbert.cu#L31-L44. This PR modifies the behavior such that the invalid memory access is avoided and instead the context will perform a check on the simulation during what was previously the `_verify_box` check. 

Note that this only triggers if `multiple_steps` returns coordinates, so doing `multiple_steps(5, store_x_interval=6)` will not trigger the failure. This is consistent with the box check we do regarding the size of the box relative to the cutoff.

## Benchmarks
Cuda Arch 86
Nvidia A10

The only real difference is in the potentials benchmarks which were incorrect due to the frames coming from an unstable simulation that was missed until this change.

### Master
```
dhfr-apo: N=23558 speed: 688.76ns/day dt: 2.5fs (ran 100000 steps in 31.56s)
dhfr-apo-barostat-interval-25: N=23558 speed: 621.79ns/day dt: 2.5fs (ran 100000 steps in 34.93s)

hif2a-apo: N=8805 speed: 1183.93ns/day dt: 2.5fs (ran 100000 steps in 18.50s)
hif2a-apo-barostat-interval-25: N=8805 speed: 1015.33ns/day dt: 2.5fs (ran 100000 steps in 21.47s)
hif2a-rbfe-barostat-interval-25: N=8840 speed: 867.45ns/day dt: 2.5fs (ran 100000 steps in 25.10s)
hif2a-rbfe-local: N=8840 speed: 1419.63ns/day dt: 2.5fs (ran 100000 steps in 15.40s)
hif2a-rbfe-barostat-interval-25-water-sampling-interval-400: N=8840 speed: 811.24ns/day dt: 2.5fs (ran 100000 steps in 26.83s)

solvent-apo: N=6282 speed: 1619.75ns/day dt: 2.5fs (ran 100000 steps in 13.56s)
solvent-apo-barostat-interval-25: N=6282 speed: 1298.72ns/day dt: 2.5fs (ran 100000 steps in 16.86s)
solvent-rbfe-barostat-interval-25: N=6317 speed: 1090.48ns/day dt: 2.5fs (ran 100000 steps in 20.00s)
solvent-rbfe-local: N=6317 speed: 1525.73ns/day dt: 2.5fs (ran 100000 steps in 14.35s)
ahfe-barostat-interval-15: N=6316 speed: 1030.64ns/day dt: 2.5fs (ran 100000 steps in 21.25s)
ahfe-local: N=6316 speed: 1445.78ns/day dt: 2.5fs (ran 100000 steps in 15.13s)

vacuum-rbfe: N=35 speed: 9440.39ns/day dt: 2.5fs (ran 100000 steps in 2.47s)

NonbondedInteractionGroup_f32: N=8840 Frames=1000 Params=5 speed: 1212.53 executions/seconds (ran 10000 potentials in 8.25s) du_dp=True, du_dx=True, u=True
NonbondedInteractionGroup_f64: N=8840 Frames=1000 Params=5 speed: 514.90 executions/seconds (ran 10000 potentials in 19.42s) du_dp=True, du_dx=True, u=True
HarmonicBond_f32: N=8840 Frames=1000 Params=5 speed: 1511.05 executions/seconds (ran 10000 potentials in 6.62s) du_dp=True, du_dx=True, u=True
HarmonicBond_f64: N=8840 Frames=1000 Params=5 speed: 1518.65 executions/seconds (ran 10000 potentials in 6.59s) du_dp=True, du_dx=True, u=True
HarmonicAngleStable_f32: N=8840 Frames=1000 Params=5 speed: 1508.51 executions/seconds (ran 10000 potentials in 6.63s) du_dp=True, du_dx=True, u=True
HarmonicAngleStable_f64: N=8840 Frames=1000 Params=5 speed: 1452.11 executions/seconds (ran 10000 potentials in 6.89s) du_dp=True, du_dx=True, u=True
PeriodicTorsion_f32: N=8840 Frames=1000 Params=5 speed: 1479.11 executions/seconds (ran 10000 potentials in 6.76s) du_dp=True, du_dx=True, u=True
PeriodicTorsion_f64: N=8840 Frames=1000 Params=5 speed: 1415.43 executions/seconds (ran 10000 potentials in 7.07s) du_dp=True, du_dx=True, u=True
ChiralAtomRestraint_f32: N=8840 Frames=1000 Params=5 speed: 1732.45 executions/seconds (ran 10000 potentials in 5.77s) du_dp=True, du_dx=True, u=True
ChiralAtomRestraint_f64: N=8840 Frames=1000 Params=5 speed: 1694.33 executions/seconds (ran 10000 potentials in 5.90s) du_dp=True, du_dx=True, u=True
NonbondedPairListPrecomputed_f32: N=8840 Frames=1000 Params=5 speed: 1595.62 executions/seconds (ran 10000 potentials in 6.27s) du_dp=True, du_dx=True, u=True
NonbondedPairListPrecomputed_f64: N=8840 Frames=1000 Params=5 speed: 1552.53 executions/seconds (ran 10000 potentials in 6.44s) du_dp=True, du_dx=True, u=True
Nonbonded_f32: N=8840 Frames=1000 Params=5 speed: 593.72 executions/seconds (ran 10000 potentials in 16.84s) du_dp=True, du_dx=True, u=True
Nonbonded_f64: N=8840 Frames=1000 Params=5 speed: 31.63 executions/seconds (ran 10000 potentials in 316.13s) du_dp=True, du_dx=True, u=True
NonbondedInteractionGroup_f32: N=8840 Frames=1000 Params=5 speed: 1129.33 executions/seconds (ran 10000 potentials in 8.85s) du_dp=True, du_dx=True, u=True
NonbondedInteractionGroup_f64: N=8840 Frames=1000 Params=5 speed: 485.14 executions/seconds (ran 10000 potentials in 20.61s) du_dp=True, du_dx=True, u=True
```

### Changes
```
dhfr-apo: N=23558 speed: 686.58ns/day dt: 2.5fs (ran 100000 steps in 31.65s)
dhfr-apo-barostat-interval-25: N=23558 speed: 622.96ns/day dt: 2.5fs (ran 100000 steps in 34.86s)
building a protein system with 1758 protein atoms and 7047 water atoms

hif2a-apo: N=8805 speed: 1185.23ns/day dt: 2.5fs (ran 100000 steps in 18.54s)
hif2a-apo-barostat-interval-25: N=8805 speed: 1017.58ns/day dt: 2.5fs (ran 100000 steps in 21.46s)
hif2a-rbfe-barostat-interval-25: N=8840 speed: 875.16ns/day dt: 2.5fs (ran 100000 steps in 24.91s)
hif2a-rbfe-local: N=8840 speed: 1432.79ns/day dt: 2.5fs (ran 100000 steps in 15.26s)
hif2a-rbfe-barostat-interval-25-water-sampling-interval-400: N=8840 speed: 814.29ns/day dt: 2.5fs (ran 100000 steps in 26.73s)

solvent-apo: N=6282 speed: 1628.22ns/day dt: 2.5fs (ran 100000 steps in 13.56s)
solvent-apo-barostat-interval-25: N=6282 speed: 1301.11ns/day dt: 2.5fs (ran 100000 steps in 16.80s)
solvent-rbfe-barostat-interval-25: N=6317 speed: 1093.61ns/day dt: 2.5fs (ran 100000 steps in 19.95s)
solvent-rbfe-local: N=6317 speed: 1543.40ns/day dt: 2.5fs (ran 100000 steps in 14.19s)
ahfe-barostat-interval-15: N=6316 speed: 1034.51ns/day dt: 2.5fs (ran 100000 steps in 21.08s)
ahfe-local: N=6316 speed: 1463.71ns/day dt: 2.5fs (ran 100000 steps in 14.95s)

vacuum-rbfe: N=35 speed: 9787.45ns/day dt: 2.5fs (ran 100000 steps in 2.39s)
building a protein system with 1758 protein atoms and 7047 water atoms

NonbondedInteractionGroup_f32: N=8840 Frames=1000 Params=5 speed: 1129.22 executions/seconds (ran 10000 potentials in 8.86s) du_dp=True, du_dx=True, u=True
NonbondedInteractionGroup_f64: N=8840 Frames=1000 Params=5 speed: 505.07 executions/seconds (ran 10000 potentials in 19.80s) du_dp=True, du_dx=True, u=True
HarmonicBond_f32: N=8840 Frames=1000 Params=5 speed: 1432.29 executions/seconds (ran 10000 potentials in 6.98s) du_dp=True, du_dx=True, u=True
HarmonicBond_f64: N=8840 Frames=1000 Params=5 speed: 1423.94 executions/seconds (ran 10000 potentials in 7.02s) du_dp=True, du_dx=True, u=True
HarmonicAngleStable_f32: N=8840 Frames=1000 Params=5 speed: 1387.57 executions/seconds (ran 10000 potentials in 7.21s) du_dp=True, du_dx=True, u=True
HarmonicAngleStable_f64: N=8840 Frames=1000 Params=5 speed: 1374.45 executions/seconds (ran 10000 potentials in 7.28s) du_dp=True, du_dx=True, u=True
PeriodicTorsion_f32: N=8840 Frames=1000 Params=5 speed: 1395.36 executions/seconds (ran 10000 potentials in 7.17s) du_dp=True, du_dx=True, u=True
PeriodicTorsion_f64: N=8840 Frames=1000 Params=5 speed: 1367.47 executions/seconds (ran 10000 potentials in 7.31s) du_dp=True, du_dx=True, u=True
ChiralAtomRestraint_f32: N=8840 Frames=1000 Params=5 speed: 1587.83 executions/seconds (ran 10000 potentials in 6.30s) du_dp=True, du_dx=True, u=True
ChiralAtomRestraint_f64: N=8840 Frames=1000 Params=5 speed: 1575.06 executions/seconds (ran 10000 potentials in 6.35s) du_dp=True, du_dx=True, u=True
NonbondedPairListPrecomputed_f32: N=8840 Frames=1000 Params=5 speed: 1563.16 executions/seconds (ran 10000 potentials in 6.40s) du_dp=True, du_dx=True, u=True
NonbondedPairListPrecomputed_f64: N=8840 Frames=1000 Params=5 speed: 1536.73 executions/seconds (ran 10000 potentials in 6.51s) du_dp=True, du_dx=True, u=True
Nonbonded_f32: N=8840 Frames=1000 Params=5 speed: 892.92 executions/seconds (ran 10000 potentials in 11.20s) du_dp=True, du_dx=True, u=True
Nonbonded_f64: N=8840 Frames=1000 Params=5 speed: 98.37 executions/seconds (ran 10000 potentials in 101.66s) du_dp=True, du_dx=True, u=True
NonbondedInteractionGroup_f32: N=8840 Frames=1000 Params=5 speed: 1132.83 executions/seconds (ran 10000 potentials in 8.83s) du_dp=True, du_dx=True, u=True
NonbondedInteractionGroup_f64: N=8840 Frames=1000 Params=5 speed: 457.80 executions/seconds (ran 10000 potentials in 21.84s) du_dp=True, du_dx=True, u=True
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f6356ea8cbc7`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.