# Issue

**Title:** Update `nequip-compile` CLI help formatting

## Description

Small update for formatting of the `nequip-compile -h` output, including:
- Giving the default value of tf32
- Showing which parameters are required (in a separate subsection)

Previous:
```
(pytorch_2.8.0) FasRC: OpenEquivariance > nequip-compile -h
usage: nequip-compile [-h] --mode {torchscript,aotinductor} --device DEVICE [--model MODEL] [--tf32 | --no-tf32] [--modifiers MODIFIERS [MODIFIERS ...]] [--target {pair_nequip,ase}]
                      [--input-fields INPUT_FIELDS [INPUT_FIELDS ...]] [--output-fields OUTPUT_FIELDS [OUTPUT_FIELDS ...]] [--data-path DATA_PATH] [--num-frames NUM_FRAMES] [--num-edges NUM_EDGES]
                      [--num-nodes NUM_NODES] [--inductor-configs INDUCTOR_CONFIGS [INDUCTOR_CONFIGS ...]]
                      input_path output_path

Compiles NequIP/Allegro models from checkpoint or package files.

positional arguments:
  input_path            path to a checkpoint model or packaged model file
  output_path           path to write compiled model file. NOTE: a `.nequip.pth` extension is required if `--mode torchscript` is used and a `.nequip.pt2` extension is required if `--mode aotinductor` is used

options:
  -h, --help            show this help message and exit
  --mode {torchscript,aotinductor}
                        whether to use `torchscript` or `aotinductor` to compile the model
  --device DEVICE       device to run the model on
  --model MODEL         name of model to compile -- this option is only relevant when using multiple models (default: sole_model, meant to work for the conventional single model case)
  --tf32, --no-tf32     whether to use TF32 or not
  --modifiers MODIFIERS [MODIFIERS ...]
                        modifiers to apply to the model before compiling
  --target {pair_nequip,ase}
                        target application for AOT export (`input-fields` and `output-fields` need not be specified if `target` is specified)
  --input-fields INPUT_FIELDS [INPUT_FIELDS ...]
                        input fields to the model for export
  --output-fields OUTPUT_FIELDS [OUTPUT_FIELDS ...]
                        output fields of the model for export
  --data-path DATA_PATH
                        path to data (with realistic shapes) for compilation (if unspecified, training data will be used for compilation)
  --num-frames NUM_FRAMES
                        bounds for num-frames in format `min,max` or `static` (default: 2,inf)
  --num-edges NUM_EDGES
                        bounds for num-edges in format `min,max` or `static` (default: 2,inf)
  --num-nodes NUM_NODES
                        bounds for num-nodes in format `min,max` or `static` (default: 2,inf)
  --inductor-configs INDUCTOR_CONFIGS [INDUCTOR_CONFIGS ...]
                        options for AOTInductor (default: {})
```

Now:
```
❯ nequip-compile -h
usage: nequip-compile [-h] --mode {torchscript,aotinductor} --device DEVICE [--model MODEL] [--tf32 | --no-tf32] [--modifiers MODIFIERS [MODIFIERS ...]] [--target {pair_nequip,ase}] [--input-fields INPUT_FIELDS [INPUT_FIELDS ...]]
                      [--output-fields OUTPUT_FIELDS [OUTPUT_FIELDS ...]] [--data-path DATA_PATH] [--num-frames NUM_FRAMES] [--num-edges NUM_EDGES] [--num-nodes NUM_NODES] [--inductor-configs INDUCTOR_CONFIGS [INDUCTOR_CONFIGS ...]]
                      input_path output_path

Compiles NequIP/Allegro models from checkpoint or package files.

positional arguments:
  input_path            path to a checkpoint model or packaged model file
  output_path           path to write compiled model file. NOTE: a `.nequip.pth` extension is required if `--mode torchscript` is used and a `.nequip.pt2` extension is required if `--mode aotinductor` is used

options:
  -h, --help            show this help message and exit
  --model MODEL         name of model to compile -- this option is only relevant when using multiple models (default: sole_model, meant to work for the conventional single model case)
  --tf32, --no-tf32     whether to use TF32 or not (default: False)
  --modifiers MODIFIERS [MODIFIERS ...]
                        modifiers to apply to the model before compiling
  --target {pair_nequip,ase}
                        target application for AOT export (`input-fields` and `output-fields` need not be specified if `target` is specified)
  --input-fields INPUT_FIELDS [INPUT_FIELDS ...]
                        input fields to the model for export
  --output-fields OUTPUT_FIELDS [OUTPUT_FIELDS ...]
                        output fields of the model for export
  --data-path DATA_PATH
                        path to data (with realistic shapes) for compilation (if unspecified, training data will be used for compilation)
  --num-frames NUM_FRAMES
                        bounds for num-frames in format `min,max` or `static` (default: 2,inf)
  --num-edges NUM_EDGES
                        bounds for num-edges in format `min,max` or `static` (default: 2,inf)
  --num-nodes NUM_NODES
                        bounds for num-nodes in format `min,max` or `static` (default: 2,inf)
  --inductor-configs INDUCTOR_CONFIGS [INDUCTOR_CONFIGS ...]
                        options for AOTInductor (default: {})

required arguments:
  --mode {torchscript,aotinductor}
                        whether to use `torchscript` or `aotinductor` to compile the model
  --device DEVICE       device to run the model on
  ```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1e8e7bbd01fe`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.