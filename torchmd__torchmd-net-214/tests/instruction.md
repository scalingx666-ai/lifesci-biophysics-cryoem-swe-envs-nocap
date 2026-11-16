# Issue

**Title:** Make OutputModel aware of CUDA graph capturing.

## Description

The reduction in OutputModel requires knowing the number of batches (computed as batch.max()+1) to be able to produce a tensor with a per-batch property (a.i. the total energy). 
This operation is inherently blocking and thus cannot be called during CUDA graph capture.
 
One way to go around this limitation is to make the OutputModel store the number of batches each time it is called.
Then, if the module is called in capture mode (which can be detected with `torch.cuda.is_current_stream_capturing()`) the previously stored value is used instead.
This makes it so that the number of batches of a captured model cannot be larger than the one used in the call previous to capture. However, this limitation is proper to CUDA graphs and thus I do not consider it an additional limitation.

This PR implements this strategy, warning the user about this when they capture the model and raising if the model is being called without warming up first.
Possible alternatives:
1. Adding a new parameter to TorchMD_Net (like n_batches). I think this is more error prone.
2. Computing  and storing batch.max()+1 during capture without including it into the graph. I do not think this is possible sadly.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e964a72b3d0b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.