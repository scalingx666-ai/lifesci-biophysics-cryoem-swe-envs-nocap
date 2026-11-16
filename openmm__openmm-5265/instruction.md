# Issue

**Title:** Created ExpandedEnsembleSampler

## Description

This is the third piece of #5235.  It introduces a class for serial sampling of thermodynamic states with Gibbs sampling.

Using it is very similar to replica exchange.  In the simplest case, you just create a sampler specifying the Simulation and list of states.

```python
sampler = ExpandedEnsembleSampler(states, simulation, interval)
```

Then run your simulation as usual.  Since it's all just a single trajectory, it doesn't have the same issues with reporting that replica exchange does.  For the most part, you can just use standard reporters.  You do need to record what state it was in at each time.  The sampler can write that information to a log file for you.

This class mostly makes the SimulatedTempering class obsolete, since it can do all the same things and also more.  In a future PR, I plan to turn SimulatedTempering into a thin wrapper around this class.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `46376ea3e4d0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.