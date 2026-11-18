# Issue

**Title:** Unified synapses and more options

## Description

This PR unifies our synapses. It also largely improves their documentation. More generally, it introduces a larger and more flexible set of synapse dynamics:
```python
from jaxley.synapses import CurrentSynapse, ConductanceSynapse, DynamicSynapse, IonotropicSynapse
```


This PR also removes the `TanhConductanceSynapse`. Replace it with:
```python
import jax.numpy as jnp
from jaxley.synapses import ConductanceSynapse
connect(..., ConductanceSynapse(jnp.tanh))
```

And it removes the `TanhRateSynapse`. Replace it with:
```python
import jax.numpy as jnp
from jaxley.synapses import CurrentSynapse
connect(..., CurrentSynapse(jnp.tanh))
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3879557253ca`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.