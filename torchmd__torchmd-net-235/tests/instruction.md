# Issue

**Title:** Preload small datasets to memory in Custom dataset

## Description

While replicating results from the [torchmd-protein-thermodynamics repository](https://github.com/torchmd/torchmd-protein-thermodynamics), I experienced sluggish training speeds and low GPU usage (meaning sitting at 0% and briefly going to 100% at each iteration) using the following configuration file:
```yaml
activation: tanh
batch_size: 1024
inference_batch_size: 1024
dataset: Custom
coord_files: "chignolin_ca_coords.npy"
embed_files: "chignolin_ca_embeddings.npy"
force_files: "chignolin_ca_deltaforces.npy"
cutoff_upper: 12.0
cutoff_lower: 3.0
derivative: true
early_stopping_patience: 30
embedding_dimension: 128
lr: 0.0005
lr_factor: 0.8
lr_min: 1.0e-06
lr_patience: 10
lr_warmup_steps: 0
model: graph-network
neighbor_embedding: false
ngpus: -1
num_epochs: 100
num_layers: 4
num_nodes: 1
num_rbf: 18
num_workers: 4
rbf_type: expnorm
save_interval: 2
seed: 1
test_interval: 2
test_size: 0.1
trainable_rbf: true
val_size: 0.05
weight_decay: 0.0
```
The referenced files take approx 300mb.
Playing around with num_workers and batch size did not help.
Upon investigation, the issue arose from the Custom dataset's I/O-bound get method, which reads from disk every time it is invoked, causing the low GPU usage and slow training.

To resolve this, I implemented a preloading feature that loads the complete dataset into system memory if its size is below a user-configurable threshold, set by default at 1GB. The data is stored as PyTorch tensors, facilitating compatibility with multi-threaded data loaders (num_workers). Notably, this approach does not inflate RAM usage when increasing the number of workers.

This optimization led to a x20 speedup in training time for this specific setup.

OTOH I tweaked the DataLoader options a bit.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2c2b5f059b89`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.