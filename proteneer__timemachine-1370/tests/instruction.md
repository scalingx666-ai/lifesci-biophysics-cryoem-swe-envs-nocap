# Issue

**Title:** Release GIL on pybind calls to Context

## Description

* Contexts now construct a new stream to avoid competing for the default stream
* Enables us to use threading to run multiple Context's in parallel
* Modifies `multiple_steps`, `multiple_steps_local` and `multiple_steps_local_selection` to take in a host buffer to reduce memory copying, which provides a 5% speed up to Vacuum simulations (indicating we are CPU limited here by some amount)
* Not as much of a gain in performance as using [MPS](https://docs.nvidia.com/deploy/mps/), but can get within 10% of the maximum performance, also can parallelize in notebooks.

# Benchmarks
A10, Cuda Arch 8.6

Vacuum is 5% faster, else everything else appears to be within variation. 

## Master
```
dhfr-apo: N=23558 speed: 694.91ns/day dt: 2.5fs (ran 100000 steps in 31.27s)
dhfr-apo-barostat-interval-25: N=23558 speed: 629.80ns/day dt: 2.5fs (ran 100000 steps in 34.49s)

hif2a-apo: N=8805 speed: 1215.03ns/day dt: 2.5fs (ran 100000 steps in 17.97s)
hif2a-apo-barostat-interval-25: N=8805 speed: 1037.88ns/day dt: 2.5fs (ran 100000 steps in 21.01s)
hif2a-rbfe-barostat-interval-25: N=8840 speed: 869.26ns/day dt: 2.5fs (ran 100000 steps in 25.05s)
hif2a-rbfe-local: N=8840 speed: 1267.31ns/day dt: 2.5fs (ran 100000 steps in 17.23s)
hif2a-rbfe-barostat-interval-25-water-sampling-interval-400: N=8840 speed: 805.75ns/day dt: 2.5fs (ran 100000 steps in 27.02s)

solvent-apo: N=6282 speed: 1678.29ns/day dt: 2.5fs (ran 100000 steps in 13.06s)
solvent-apo-barostat-interval-25: N=6282 speed: 1336.22ns/day dt: 2.5fs (ran 100000 steps in 16.44s)
solvent-rbfe-barostat-interval-25: N=6317 speed: 1082.85ns/day dt: 2.5fs (ran 100000 steps in 20.15s)
solvent-rbfe-local: N=6317 speed: 1354.26ns/day dt: 2.5fs (ran 100000 steps in 16.15s)

vacuum-rbfe: N=35 speed: 9107.07ns/day dt: 2.5fs (ran 100000 steps in 2.55s)
```

## Changes
```
dhfr-apo: N=23558 speed: 696.28ns/day dt: 2.5fs (ran 100000 steps in 31.22s)
dhfr-apo-barostat-interval-25: N=23558 speed: 632.08ns/day dt: 2.5fs (ran 100000 steps in 34.37s)

hif2a-apo: N=8805 speed: 1210.60ns/day dt: 2.5fs (ran 100000 steps in 17.99s)
hif2a-apo-barostat-interval-25: N=8805 speed: 1036.43ns/day dt: 2.5fs (ran 100000 steps in 21.00s)
hif2a-rbfe-barostat-interval-25: N=8840 speed: 862.80ns/day dt: 2.5fs (ran 100000 steps in 25.19s)
hif2a-rbfe-local: N=8840 speed: 1269.70ns/day dt: 2.5fs (ran 100000 steps in 17.15s)
hif2a-rbfe-barostat-interval-25-water-sampling-interval-400: N=8840 speed: 805.40ns/day dt: 2.5fs (ran 100000 steps in 26.99s)

solvent-apo: N=6282 speed: 1674.43ns/day dt: 2.5fs (ran 100000 steps in 13.04s)
solvent-apo-barostat-interval-25: N=6282 speed: 1337.73ns/day dt: 2.5fs (ran 100000 steps in 16.30s)
solvent-rbfe-barostat-interval-25: N=6317 speed: 1082.23ns/day dt: 2.5fs (ran 100000 steps in 20.12s)
solvent-rbfe-local: N=6317 speed: 1363.13ns/day dt: 2.5fs (ran 100000 steps in 16.00s)

vacuum-rbfe: N=35 speed: 9633.95ns/day dt: 2.5fs (ran 100000 steps in 2.39s)
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8e46cf807cc9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.