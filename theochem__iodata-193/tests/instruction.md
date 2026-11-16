# Issue

**Title:** Update license_file in conda.recipe meta.yaml

## Description

This should fix the latest failed conda build caused by:

```
Traceback (most recent call last):
  File "/home/travis/cache/miniconda3/bin/conda-build", line 11, in <module>
    sys.exit(main())
  File "/home/travis/cache/miniconda3/lib/python3.8/site-packages/conda_build/cli/main_build.py", line 469, in main
    execute(sys.argv[1:])
  File "/home/travis/cache/miniconda3/lib/python3.8/site-packages/conda_build/cli/main_build.py", line 458, in execute
    outputs = api.build(args.recipe, post=args.post, build_only=args.build_only,
  File "/home/travis/cache/miniconda3/lib/python3.8/site-packages/conda_build/api.py", line 208, in build
    return build_tree(absolute_recipes, config, stats, build_only=build_only, post=post,
  File "/home/travis/cache/miniconda3/lib/python3.8/site-packages/conda_build/build.py", line 2339, in build_tree
    packages_from_this = build(metadata, stats,
  File "/home/travis/cache/miniconda3/lib/python3.8/site-packages/conda_build/build.py", line 1643, in build
    newly_built_packages = bundlers[pkg_type](output_d, m, env, stats)
  File "/home/travis/cache/miniconda3/lib/python3.8/site-packages/conda_build/build.py", line 1015, in bundle_conda
    output['checksums'] = create_info_files(metadata, files, prefix=metadata.config.host_prefix)
  File "/home/travis/cache/miniconda3/lib/python3.8/site-packages/conda_build/build.py", line 709, in create_info_files
    copy_license(m)
  File "/home/travis/cache/miniconda3/lib/python3.8/site-packages/conda_build/build.py", line 402, in copy_license
    raise ValueError("License file given in about/license_file ({}) does not exist in "
ValueError: License file given in about/license_file (/home/travis/build/theochem/iodata/tools/conda.recipe/LICENSE.txt) does not exist in source root dir or in recipe root dir (with meta.yaml)
The command "if [[ "$TRAVIS_PULL_REQUEST" == "false" ]]; then python3 -m roberto robot; else python3 -m roberto; fi" exited with 1.
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e527a81d400f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.