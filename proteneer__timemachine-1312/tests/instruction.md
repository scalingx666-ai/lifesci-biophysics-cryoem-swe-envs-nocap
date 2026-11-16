# Issue

**Title:** Use fast real to int64 conversion for all recent GPUs

## Description

* Significantly faster on RTX 4090, only 25% faster on the L4s

# Benchmarks
RTX 4090
Cuda Arch 8.9

* DHFR: 50% faster
* Hif2a: 30-40% faster
* Solvent: 25% faster
* Vacuum: 2% slower (this doesn't seem converged and 18us a day, so how much do we care about this getting slower?)

### Caveats
* Run with a GUI, so not super converged but seems pretty clearly faster

## Master
```
dhfr-apo: N=23558 speed: 1010.03ns/day dt: 2.5fs (ran 100000 steps in 21.39s)
dhfr-apo-barostat-interval-25: N=23558 speed: 943.35ns/day dt: 2.5fs (ran 100000 steps in 22.90s)
building a protein system with 1758 protein atoms and 7047 water atoms

hif2a-apo: N=8805 speed: 1710.75ns/day dt: 2.5fs (ran 100000 steps in 12.63s)
hif2a-apo-barostat-interval-25: N=8805 speed: 1449.09ns/day dt: 2.5fs (ran 100000 steps in 14.91s)
hif2a-rbfe-barostat-interval-25: N=8840 speed: 1071.11ns/day dt: 2.5fs (ran 100000 steps in 20.17s)
hif2a-rbfe-local: N=8840 speed: 1692.30ns/day dt: 2.5fs (ran 100000 steps in 12.77s)
hif2a-rbfe-barostat-interval-25-water-sampling-interval-400: N=8840 speed: 1037.14ns/day dt: 2.5fs (ran 100000 steps in 20.83s)

solvent-apo: N=6282 speed: 2098.06ns/day dt: 2.5fs (ran 100000 steps in 10.30s)
solvent-apo-barostat-interval-25: N=6282 speed: 1910.43ns/day dt: 2.5fs (ran 100000 steps in 11.31s)
solvent-rbfe-barostat-interval-25: N=6317 speed: 1489.62ns/day dt: 2.5fs (ran 100000 steps in 14.50s)
solvent-rbfe-local: N=6317 speed: 1980.73ns/day dt: 2.5fs (ran 100000 steps in 10.91s)

vacuum-rbfe: N=35 speed: 18594.25ns/day dt: 2.5fs (ran 100000 steps in 1.16s)
building a protein system with 1758 protein atoms and 7047 water atoms

NonbondedInteractionGroup_f32: N=8840 Frames=1000 Params=5 speed: 1143.75 executions/seconds (ran 10000 potentials in 8.74s) du_dp=True, du_dx=True, u=True
NonbondedInteractionGroup_f64: N=8840 Frames=1000 Params=5 speed: 650.69 executions/seconds (ran 10000 potentials in 15.37s) du_dp=True, du_dx=True, u=True
HarmonicBond_f32: N=8840 Frames=1000 Params=5 speed: 1480.25 executions/seconds (ran 10000 potentials in 6.76s) du_dp=True, du_dx=True, u=True
HarmonicBond_f64: N=8840 Frames=1000 Params=5 speed: 1477.87 executions/seconds (ran 10000 potentials in 6.77s) du_dp=True, du_dx=True, u=True
HarmonicAngleStable_f32: N=8840 Frames=1000 Params=5 speed: 1434.86 executions/seconds (ran 10000 potentials in 6.97s) du_dp=True, du_dx=True, u=True
HarmonicAngleStable_f64: N=8840 Frames=1000 Params=5 speed: 1421.22 executions/seconds (ran 10000 potentials in 7.04s) du_dp=True, du_dx=True, u=True
PeriodicTorsion_f32: N=8840 Frames=1000 Params=5 speed: 1436.35 executions/seconds (ran 10000 potentials in 6.96s) du_dp=True, du_dx=True, u=True
PeriodicTorsion_f64: N=8840 Frames=1000 Params=5 speed: 1424.48 executions/seconds (ran 10000 potentials in 7.02s) du_dp=True, du_dx=True, u=True
ChiralAtomRestraint_f32: N=8840 Frames=1000 Params=5 speed: 1670.08 executions/seconds (ran 10000 potentials in 5.99s) du_dp=True, du_dx=True, u=True
ChiralAtomRestraint_f64: N=8840 Frames=1000 Params=5 speed: 1657.42 executions/seconds (ran 10000 potentials in 6.03s) du_dp=True, du_dx=True, u=True
NonbondedPairListPrecomputed_f32: N=8840 Frames=1000 Params=5 speed: 1635.62 executions/seconds (ran 10000 potentials in 6.11s) du_dp=True, du_dx=True, u=True
NonbondedPairListPrecomputed_f64: N=8840 Frames=1000 Params=5 speed: 1617.85 executions/seconds (ran 10000 potentials in 6.18s) du_dp=True, du_dx=True, u=True
Nonbonded_f32: N=8840 Frames=1000 Params=5 speed: 978.09 executions/seconds (ran 10000 potentials in 10.22s) du_dp=True, du_dx=True, u=True
Nonbonded_f64: N=8840 Frames=1000 Params=5 speed: 219.21 executions/seconds (ran 10000 potentials in 45.62s) du_dp=True, du_dx=True, u=True
SummedPotential(NonbondedInteractionGroup, NonbondedInteractionGroup)_f32: N=8840 Frames=1000 Params=5 speed: 896.97 executions/seconds (ran 10000 potentials in 11.15s) du_dp=True, du_dx=True, u=True
SummedPotential(NonbondedInteractionGroup, NonbondedInteractionGroup)_f64: N=8840 Frames=1000 Params=5 speed: 527.77 executions/seconds (ran 10000 potentials in 18.95s) du_dp=True, du_dx=True, u=True
```

## PR
```
dhfr-apo: N=23558 speed: 1500.35ns/day dt: 2.5fs (ran 100000 steps in 14.40s)
dhfr-apo-barostat-interval-25: N=23558 speed: 1358.20ns/day dt: 2.5fs (ran 100000 steps in 15.91s)
building a protein system with 1758 protein atoms and 7047 water atoms

hif2a-apo: N=8805 speed: 2424.14ns/day dt: 2.5fs (ran 100000 steps in 8.91s)
hif2a-apo-barostat-interval-25: N=8805 speed: 2002.71ns/day dt: 2.5fs (ran 100000 steps in 10.79s)
hif2a-rbfe-barostat-interval-25: N=8840 speed: 1438.11ns/day dt: 2.5fs (ran 100000 steps in 15.02s)
hif2a-rbfe-local: N=8840 speed: 1875.33ns/day dt: 2.5fs (ran 100000 steps in 11.52s)
hif2a-rbfe-barostat-interval-25-water-sampling-interval-400: N=8840 speed: 1363.70ns/day dt: 2.5fs (ran 100000 steps in 15.84s)

solvent-apo: N=6282 speed: 2829.15ns/day dt: 2.5fs (ran 100000 steps in 7.64s)
solvent-apo-barostat-interval-25: N=6282 speed: 2505.44ns/day dt: 2.5fs (ran 100000 steps in 8.62s)
solvent-rbfe-barostat-interval-25: N=6317 speed: 1954.64ns/day dt: 2.5fs (ran 100000 steps in 11.05s)
solvent-rbfe-local: N=6317 speed: 2229.19ns/day dt: 2.5fs (ran 100000 steps in 9.69s)
vacuum-rbfe: N=35 speed: 18268.92ns/day dt: 2.5fs (ran 100000 steps in 1.18s)
building a protein system with 1758 protein atoms and 7047 water atoms

NonbondedInteractionGroup_f32: N=8840 Frames=1000 Params=5 speed: 1162.35 executions/seconds (ran 10000 potentials in 8.60s) du_dp=True, du_dx=True, u=True
NonbondedInteractionGroup_f64: N=8840 Frames=1000 Params=5 speed: 650.21 executions/seconds (ran 10000 potentials in 15.38s) du_dp=True, du_dx=True, u=True
HarmonicBond_f32: N=8840 Frames=1000 Params=5 speed: 1478.47 executions/seconds (ran 10000 potentials in 6.76s) du_dp=True, du_dx=True, u=True
HarmonicBond_f64: N=8840 Frames=1000 Params=5 speed: 1478.07 executions/seconds (ran 10000 potentials in 6.77s) du_dp=True, du_dx=True, u=True
HarmonicAngleStable_f32: N=8840 Frames=1000 Params=5 speed: 1432.26 executions/seconds (ran 10000 potentials in 6.98s) du_dp=True, du_dx=True, u=True
HarmonicAngleStable_f64: N=8840 Frames=1000 Params=5 speed: 1420.75 executions/seconds (ran 10000 potentials in 7.04s) du_dp=True, du_dx=True, u=True
PeriodicTorsion_f32: N=8840 Frames=1000 Params=5 speed: 1438.94 executions/seconds (ran 10000 potentials in 6.95s) du_dp=True, du_dx=True, u=True
PeriodicTorsion_f64: N=8840 Frames=1000 Params=5 speed: 1424.51 executions/seconds (ran 10000 potentials in 7.02s) du_dp=True, du_dx=True, u=True
ChiralAtomRestraint_f32: N=8840 Frames=1000 Params=5 speed: 1670.13 executions/seconds (ran 10000 potentials in 5.99s) du_dp=True, du_dx=True, u=True
ChiralAtomRestraint_f64: N=8840 Frames=1000 Params=5 speed: 1658.82 executions/seconds (ran 10000 potentials in 6.03s) du_dp=True, du_dx=True, u=True
NonbondedPairListPrecomputed_f32: N=8840 Frames=1000 Params=5 speed: 1634.76 executions/seconds (ran 10000 potentials in 6.12s) du_dp=True, du_dx=True, u=True
NonbondedPairListPrecomputed_f64: N=8840 Frames=1000 Params=5 speed: 1617.17 executions/seconds (ran 10000 potentials in 6.18s) du_dp=True, du_dx=True, u=True
Nonbonded_f32: N=8840 Frames=1000 Params=5 speed: 1044.73 executions/seconds (ran 10000 potentials in 9.57s) du_dp=True, du_dx=True, u=True
Nonbonded_f64: N=8840 Frames=1000 Params=5 speed: 218.68 executions/seconds (ran 10000 potentials in 45.73s) du_dp=True, du_dx=True, u=True
SummedPotential(NonbondedInteractionGroup, NonbondedInteractionGroup)_f32: N=8840 Frames=1000 Params=5 speed: 915.51 executions/seconds (ran 10000 potentials in 10.92s) du_dp=True, du_dx=True, u=True
SummedPotential(NonbondedInteractionGroup, NonbondedInteractionGroup)_f64: N=8840 Frames=1000 Params=5 speed: 526.73 executions/seconds (ran 10000 potentials in 18.99s) du_dp=True, du_dx=True, u=True
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1f8387e0b730`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.