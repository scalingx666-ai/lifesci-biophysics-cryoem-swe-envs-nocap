# Issue

**Title:** torch.compile neighbors without graph breaks

## Description

Pytorch introduced a new API to handle extensions, it is "documented" here: https://docs.google.com/document/d/1_W62p8WJOQQUzPsJYa7s701JXt0qf2OfLub2sbkHOaU/edit

It makes it possible to write meta registrations for C++ extensions, which I could not make before. With a meta registration torch.compile is able to understand custom operations. A meta registration is an implementation of the operator for the "meta" device (akin to CPU or CUDA), in which tensors only have shapes and are refered to as FakeTensor.
It is used by pytorch to gather information about the input/output shapes of an operator for compilation purposes.
 

Makes this code possible:
```python
    example_pos = 100 * torch.rand(
        50, 3, requires_grad=True, dtype=dtype, device=device
    )
    model = OptimizedDistance(
        return_vecs=True,
        loop=True,
        max_num_pairs=-50,
        include_transpose=True,
        resize_to_fit=False,
        check_errors=False,
    ).to(device)
    for _ in range(25):
        model(example_pos)
    edge_index, edge_vec, edge_distance = model(example_pos)
    model = torch.compile(
        model,
        fullgraph=True,
        backend="inductor",
        mode="reduce-overhead",
    )
    edge_index, edge_vec, edge_distance = model(example_pos)
```
Prior to this PR torch.compile had to be instructed to exclude the nieghbor extension from the operation graph:
https://github.com/torchmd/torchmd-net/blob/fae79bd9e8e96ab235c52d5e54e33c3be9e4d05d/torchmdnet/extensions/__init__.py#L116-L118

So it could not be compiled with `fullgraph=True`.

The new API starts at version 2.2.1, which is not yet in conda-forge. I made it so that the current behavior is unchanged for versions prior to it.

Still compile is not able to handle code like this, in which a particular item from a tensor is accessed.
```python
        if self.check_errors:
            assert (
                num_pairs[0] <= max_pairs
            ), f"Found num_pairs({num_pairs[0]}) > max_num_pairs({max_pairs})"
```
It can still be compiled, just not with fullgraph=True.
The general rule being "if you can capture it into a CUDA graph you can torch.compile it"

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `fae79bd9e8e9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.