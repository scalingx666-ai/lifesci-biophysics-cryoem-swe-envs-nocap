# Issue

**Title:** CUDA graph compatible neighbor backward

## Description

I modified the neighbor search backward operation so that it is compatible with graphs.
The usual transformation for these things is to replace "index selecting" operations with masked ones. For instance this:

```python
   result[mask] = a[mask] + b[mask] # Requires reductions and is thus incompatible
```
Could be transformed into

```python
   result = (a + b).masked_fill(mask, 0)
```
Sadly libtorch does not offer that many masked operations... In particular I would need ```masked_index_add```.
Given that it does not exist I hack its behavior by simply dumping all masked values to a dummy index and then trimming that dummy index:

```c++
        auto grad_positions_ = torch::zeros({num_atoms + 1, 3}, edge_vec.options());
        auto edge_index_ =
            edge_index.masked_fill(zero_mask.unsqueeze(0).expand_as(edge_index), num_atoms);
        grad_positions_.index_add_(0, edge_index_[0], result);
        grad_positions_.index_add_(0, edge_index_[1], -result);
        auto grad_positions = grad_positions_.index({Slice(0, num_atoms), Slice()});
```
While this might be inefficient when there are lots of masked values (I reckon index_add uses atomics), I believe the trade off is worth it with respect ```result[mask]```, which requires a reduction and taking into account CUDA graphs. Also, it is easily modified if and when masked_index_add is added to libtorch.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `20c5de6e7858`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.