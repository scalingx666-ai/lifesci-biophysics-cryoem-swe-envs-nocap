# Issue

**Title:** Add HIP support for the unit tests

## Description

~This is only a draft PR at present. I haven't figured out how to use libhipcxx for `cuda::std::array`.~

Update:

I have implemented `colvars_gpu::array1d` to avoid the use of libhipcxx. The unit tests can now be run on HIP. You can build the `run_colvars_test_cuda` on HIP with the following commands:
```
cd tests/functional_gpu
mkdir build
cd build
cmake ../ -DGPU_TYPE=HIP -DCMAKE_CXX_COMPILER:PATH=/opt/rocm/bin/amdclang++ -DCMAKE_PREFIX_PATH=/opt/rocm -DCMAKE_BUILD_TYPE=Release
```
and then run `make` and `make test`. The test passed on my laptop with Radeon 610M and also Frontier with MI250X (requires `-DCMAKE_HIP_ARCHITECTURES="gfx90a").

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `40b1b7ff0e22`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.