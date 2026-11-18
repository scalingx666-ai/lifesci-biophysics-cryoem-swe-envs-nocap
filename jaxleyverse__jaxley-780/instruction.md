# Issue

**Title:** AdEx Simplified Neuron Model

## Description

Implementation mentioned in #774.

Notes:
I did not enable surrogate gradient support, because it is added already in v1.0.0.
I tried to directly adept it to v1.0.0 (also adapted to the channel API etc.), but I struggled to get the pytests working, due to old jaxley_mech version.
Therefore I stuck to the pattern of izhikevich.py which is currently available in the main branch.

To update to versions 1.0.0 surrogate gradient, one would have to replace
```python
        # Check for spike and reset
        spike_occurred = (
            v >= v_threshold
        )  # ToDo: replace this with heaviside function in v1.0.0
        v = jax.lax.select(spike_occurred, v_reset, v)
        w = jax.lax.select(spike_occurred, w + b, w)
```
with
```python
        spike_occurred = heaviside(voltage - v_threshold)
        v = spike_occurred * v_reset + (1.0 - spike_occurred) * v
        w = spike_occurred * (w + b) + (1.0 - spike_occurred) * w
```

[This google colab script](https://colab.research.google.com/github/paulmyr/DD2375-HT25-Project-Course-in-High-Performance-Computing/blob/master/5_evaluation/notebooks/jaxley_test_adex_version_pullrequest.ipynb) demonstrates the model using different parameters and compares them to brian2.

Like I said, this is my first pull request to an open source project.
If I did anything wrong or have to change it, if you have any other feedback otherwise, I greatly appreciate the feedback.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `86eefb7ffb0c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.