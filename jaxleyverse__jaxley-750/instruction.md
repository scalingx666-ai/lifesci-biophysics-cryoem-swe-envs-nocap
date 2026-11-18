# Issue

**Title:** feat: verbose recordings

## Description

```python
import jaxley as jx

net = jx.Network(...)

net.cell(0).branch(0).comp(0).record()
net.IonotropicSynapse.edge(0).record("IonotropicSynapse_s")
recs = jx.integrate(net, t_max=10.0)

net.write_recordings(recs)
print(net.cell(0).branch(0).comp(0).recording())  # Shape (1, T).
print(net.IonotropicSynapse.edge(0).recording("IonotropicSynapse_s"))  # Shape (1, T).
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b192cea79c6c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.