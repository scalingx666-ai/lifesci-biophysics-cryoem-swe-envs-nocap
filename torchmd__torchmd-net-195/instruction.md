# Issue

**Title:** Make TensorNet CUDA-graph compatible

## Description

This PR is an effort to optimize TensorNet. Note that I merged here also all the changes in #186. 

I have focused on inference of small molecules (such as Alanine, 22 atoms), in which we saw the CUDA kernel launching overhead was tremendous:
![image](https://github.com/torchmd/torchmd-net/assets/13015792/b818685e-82f9-458c-b0dd-bae683520b23)
My strategy was to simply modify what was needed for ```torch.compile(backend="inductor", mode="reduce-overhead")``` to put as much as possible into a CUDA graph.
 
I replaced some blocking operations and I was able to convince TorchDynamo to put the bulk of the forward pass into a CUDA graph (orange is a graph):
![image](https://github.com/torchmd/torchmd-net/assets/13015792/0ba7e88b-e2d5-4fef-a735-829abdb50250)
 Zooming in, I am having trouble with making TorchDynamo understand the neighbor kernels:
![image](https://github.com/torchmd/torchmd-net/assets/13015792/37d11463-0537-49fc-bd1c-1e7241c821fe)
Alas, TorchDynamo refuses to graph the backwards pass. But I know it is possible to graph it, since I can do so manually.

Anyhow, I believe this is a problem that will solve itself in time as TorchDynamo improves its support for things like Dynamic shapes and extensions. And even today, the speedup is impressive.

I am not sure how to apply optimizations such as CUDA graphs for training, where shapes are dynamic. However, in those situations the changes sin this PR will just not improve things as much. TorchDynamo refuses to even process the model when used during training, complaining about dynamic shapes.

You will notice the benchmark tries to compile the model by doing this:
```python
model = torch.compile(model, backend="inductor", mode="max-autotune")
```
This line fails miserably when using the current environment, I had to use pytorch-nightly (which provides version 2.1.0):
```shell
conda install pytorch torchvision torchaudio pytorch-cuda=12.1 -c pytorch-nightly -c nvidia
```  
Which made me install the rest of the torch friends with pip:
```shell
$ pip list | grep torch
pytorch-lightning       1.6.3
torch                   2.1.0.dev20230619
torch-cluster           1.6.1
torch-geometric         2.3.1
torch-scatter           2.1.1
torchmdnet              0.2.5
torchmetrics            0.11.4
```
Luckily these compile just fine with pip.
With a little luck after this  is merged we can update the env and stay in conda-forge.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ac16c09069f3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.