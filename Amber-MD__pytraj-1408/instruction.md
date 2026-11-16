# Issue

**Title:** Add CMake build system changes

## Description

The CMake build system requires some changes to pytraj.  In CMake, the location of compiled files is decided by CMake, and is not guaranteed to be consistent.   So, you can't just use the Amber lib directory.  You *almost* had the right setup for CMake to pass in the locations via `CPPTRAJ_LIBDIR` and `CPPTRAJ_HEADERDIR`; I just had to edit `base_setup.py` so that it will use those variables when they are provided even if pytraj is being built inside Amber.

Also, I designed the CMake build system to not modify the source directory at all so as to make using Git easier, and I found the `build -b <dir>` argument to Distutils to relocate the Python built files.  However, there doesn't seem to be any way to tell Cython not to put its generated files in the source directory, at least from the command line.  Do you know if it's possible to change the Cython build directory?

Another issue I was having was the MinGW compiler throwing errors about being unable to export symbols with names corresponding to each Python file.  It [looks like](https://stackoverflow.com/questions/34689210/error-exporting-symbol-when-building-python-c-extension-in-windows) Distutils expects each Python C extension to define some sort of initialization function.  Currently, the only way to build pytraj on Windows is to screw with your Python installation: in `<python install dir>/Lib/distutils/cygwinccompiler.py`, change line 193 to `if (False and`.  I've set up the CMake system's Miniconda installer to apply this patch automatically, but it would be nice if you could create these functions so we could get away from this hack.

Finally, if you were wondering, here's a sample of the command that CMake uses to build Pytraj on Windows:
```
env --unset=AMBERHOME CPPTRAJ_HEADERDIR=<path to cpptraj headers> CPPTRAJ_LIBDIR=<directory containing libcpptraj> CC=<cmake c compiler> CXX=<cmake cxx compiler> python setup.py build_ext --no-setuptools -b <path to build dir> --disable-openmp -compiler=mingw32 -DMS_WIN64
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `cfe9ea1b7e7a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.