# Issue

**Title:** Support long argument list specified as file for all topaz commands

## Description

Use [`fromfile_prefix_chars` option in ArgumentParser](https://docs.python.org/3/library/argparse.html#fromfile-prefix-chars) to allow specifying a long list of arguments in a file.

**Example invocation before (still supported)**:

```sh
topaz denoise -o denoised_micrographs micrographs/mic1.mrc micrographs/mic2.mrc micrographs/mic3.mrc
```

**Example invocation after**:

```sh
topaz denoise -o denoised_micrographs @mics.txt
```

Where `mics.txt` has the following contents:

```
micrographs/mic1.mrc
micrographs/mic2.mrc
micrographs/mic3.mrc
```

This would be really beneficial to have when processing thousands of micrographs on systems that have a limit on the maximum number of allowed command-line arguments. It would also be good for debugging purposes e.g., show full command used by CryoSPARC Topaz wrapper instead of a truncated command that excludes the paths for legibility. 

@tbepler please let me know if you have any feedback!

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `25cb2cb8de2a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.