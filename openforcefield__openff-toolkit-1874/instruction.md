# Issue

**Title:** Lazy load `networkx` in topology modules

## Description

I was profiling some other code and found that a ~100 ms import of `networkx` happens in the toolkit whether or not it's actually used. I remember lazy-loading this in the past, but that's reflected in the current code.

This way of profiling here shows roughly a 50-100 ms speedup depending on how one squints their eyes

```shell
$ git checkout upstream/main
$ hyperfine 'python -c "from openff.toolkit import Molecule"' --prepare 'python -c "from openff.toolkit import Molecule"'
Benchmark 1: python -c "from openff.toolkit import Molecule"
  Time (mean ± σ):      1.119 s ±  0.047 s    [User: 0.909 s, System: 0.142 s]
  Range (min … max):    1.085 s …  1.241 s    10 runs

  Warning: Statistical outliers were detected. Consider re-running this benchmark on a quiet system without any interferences from other programs. It might help to use the '--warmup' or '--prepare' options.

$ git checkout lazy-load-networkx
Previous HEAD position was ef2f514f Drop py311 from conda tests (#1873)
Switched to branch 'lazy-load-networkx'
$ hyperfine 'python -c "from openff.toolkit import Molecule"' --prepare 'python -c "from openff.toolkit import Molecule"'
Benchmark 1: python -c "from openff.toolkit import Molecule"
  Time (mean ± σ):      1.050 s ±  0.048 s    [User: 0.864 s, System: 0.121 s]
  Range (min … max):    1.007 s …  1.143 s    10 runs
```

With this patch, here's a rough picture of what takes so long to run this import. There are some other obvious candidates for lazy-loading, but I'm not going for them now:

![image](https://github.com/openforcefield/openff-toolkit/assets/7935382/f0c626b3-1479-4e1b-bc0f-2a1e443147a1)

- [ ] Tag issue being addressed
- [ ] Add [tests](https://github.com/openforcefield/openff-toolkit/tree/main/openff/toolkit/_tests)
- [ ] Update docstrings/[documentation](https://github.com/openforcefield/openff-toolkit/tree/main/docs), if applicable
- [x] [Lint](https://open-forcefield-toolkit.readthedocs.io/en/latest/developing.html#style-guide) codebase
- [ ] Update [changelog](https://github.com/openforcefield/openff-toolkit/blob/main/docs/releasehistory.rst)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `60abeb473c28`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.