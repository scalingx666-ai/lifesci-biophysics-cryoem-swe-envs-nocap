# Issue

**Title:** Update LIF, implement Izhikevich and rate-based models

## Description

- simplify LIF: remove the option to use a refractory period. This makes imlementation much easier. I think it's fine to support only a minimal version here.
- reorganize LIF: voltage update is now in `update_states`. This makes it much easier to read and also allows implementing more difficult models such as Izhikevich. In addition, it supports `solver="fwd_euler"`.
- reorganize LIF: in addition, we do not perform the `Leak` within LIF, but simply rely on `Leak`. This also allows to simulate (non-leaky) integrate-and-fire neurons. New API for LIF:
```python
from jaxley.channels import Leak, Fire
cell = jx.Cell()
cell.insert(Leak())
cell.insert(Fire())
```
- add Izhikevich
- add rate-based (unit-free) neurons
- write a tutorial about all of these things
- change the FAQ about what models can be implemented
- add tests

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2809f0d7bba1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.