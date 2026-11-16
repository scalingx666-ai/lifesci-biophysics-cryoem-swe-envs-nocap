# Issue

**Title:** Created ReplicaExchangeSampler

## Description

This is a first draft of replica exchange.  To use it, you first create your Simulation as usual.  Then create a sampler:

```python
sampler = ReplicaExchangeSampler(states, simulation, steps)
```

where `states` is the list of states to sample in [the format of MultistateSampler](https://github.com/openmm/openmm/blob/1f2203f60b383794d4ff813b8e413eb2942da77b/wrappers/python/openmm/app/internal/multistatesampler.py#L33-L45) and `steps` is the number of steps to integrate for each replica on each iteration.  Then you can simulate it by calling

```python
sampler.simulate(iterations)
```

Each iteration involves integrating each replica for `steps` time steps, then attempting exchanges.  Output and analysis are done by adding reporters, much like Simulation:

```python
sampler.reporters.append(reporter)
```

But in this case, a reporter just means any function that takes a ReplicaExchangeSampler as its argument.  It gets called at the end of every iteration.  We'll need to think about what built in reporters we want to provide.  Many users will likely want to write their own as well.

Users can customize this class to use different algorithms but subclassing it and overriding `simulateReplica()` and/or `exchangeReplicas()`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1f2203f60b38`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.