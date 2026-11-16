# Issue

**Title:** Add openmp to the XFEL builder by default

## Description

This is the end of a rabbit hole of dependencies.  I thought I'd write it here in case it comes up again.  Thanks @fredericpoitevin and @nksauter.

- psana is being upgraded to psana2, which can read xtc2, the file format created by lcls2
- We found that the order of imports would cause a failure in a minimal script which connected to a psana2 data source.  This would fail:

```
import rstbx.cftbx
import psana
```

But this would pass

```
import psana
import rstbx.cftbx
```

- The problem was that `cftbx` triggers an import of `rstbx/__init__.py` which includes `omptbx_ext`, the OpenMP adapter in cctbx.  At this point we think we know what happens next.
- `omptbx/omp_or_stubs.h` includes this code:

```
#if defined(_OPENMP)
# define OMPTBX_HAVE_OMP_H
# include <omp.h>
#else
# define OMPTBX_HAVE_STUBS_H
# include <omptbx/stubs.h>
#endif
```

In other words, if OpenMP is enabled, use `omp.h`, otherwise stub out the OpenMP API.  Since the new psana2 included a library which uses OpenMP (we think), these stubs would clobber psana.

Since we can't easily swap import orders around in the larger codebase, instead making `--enable_openmp_if_possible=True` the default for the XFEL builder is the best work around.  That's already the case for the Phaser and Phenix builders so its not such a big step.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a14dd13cb7ce`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.