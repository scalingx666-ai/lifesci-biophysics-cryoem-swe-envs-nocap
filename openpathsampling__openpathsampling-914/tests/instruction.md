# Issue

**Title:** Gracious kill hook

## Description

This builds on #911 and #755. It should only be merged after them. 

This adds a `GraciousKillHook` that writes the current simulation state to storage and ends the simulation loop by raising a `GraciousKillError` when a given maximum walltime is reached.
You can also pass a custom callable as `final_call` at hook construction, in case you want to do anything else than closing the storage when the simulation is terminated.

Example usage (assuming your steps are not superfast the code below should end the simulation after something sligthly smaller than 3 min 34 s):
 ```python
 kill_hook = GraciousKillHook("3 minutes 34 seconds")
# sampler is a `PathSimulator`
sampler.attach_hook(kill_hook)
try:
    sampler.run(2000)
except GraciousKillError:
    print("Simulation ended due to maximum walltime reached")
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `08fb2bc38369`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.