# Issue

**Title:** [MRG] Add ability to record synaptic currents

## Description

and #533

As discussed with @jnsbck in #515, this PR adds `synapse_current_names` as an attribute to the base module, and then appends this to the synapse states in `_get_state_names()`. The end goal is for currents to be recordable via:

```py
net.record('IonotropicSynapse_current')
```

Some lingering thoughts:
1) Is it worth changing "current" to "i" for consistency with the channel naming conventions?
2) It took me awhile to figure out that I needed to modify the `View` class. Will it be necessary to add a `self._synapses_in_view()` property for this to work properly?
3) Add some tests that check basic assumptions of the current recording?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6474ac255aaa`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.