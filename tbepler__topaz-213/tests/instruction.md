# Issue

**Title:** fix(commands): missing arch command

## Description

`!topaz denoise3d -o ./ volumes /kaggle/working/tmp.mrc -m unet-3d-20a`

```# using device=-2 with cuda=False
# Loading model:unet-3d-20a
Traceback (most recent call last):
  File "/opt/conda/bin/topaz", line 33, in <module>
    sys.exit(load_entry_point('topaz-em==0.3.1', 'console_scripts', 'topaz')())
  File "/opt/conda/lib/python3.10/site-packages/topaz/main.py", line 148, in main
    args.func(args)
  File "/opt/conda/lib/python3.10/site-packages/topaz/commands/denoise3d.py", line 88, in main
    denoiser = Denoise3D(args.arch, use_cuda) if args.model != 'none' else None
AttributeError: 'Namespace' object has no attribute 'arch'```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ea7098d70620`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.