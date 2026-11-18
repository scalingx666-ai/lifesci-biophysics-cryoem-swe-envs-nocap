# Issue

**Title:** highlight synapse_terminals in plotting

## Description

```python
branch = jx.Branch(nseg=10)
cell = jx.Cell([branch]*3, [-1,0,0])
net = jx.Network([cell]*6)
net.compute_xyz()
net.arrange_in_layers([3, 3])
fully_connect(net[:3], net[3:], IonotropicSynapse())

net.vis()
```

looks like this again now.
![image](https://github.com/user-attachments/assets/ab433574-3ab9-4d90-99c7-03b5dbf69d4e)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a6c19ab2e74d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.