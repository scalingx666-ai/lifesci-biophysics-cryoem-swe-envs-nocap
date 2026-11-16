# Issue

**Title:** add semicolon in /src/acc/acc_ml_optimiser_impl.h for PROFILE build

## Description

Dear All, 
I was trying to install relion/4.1.0 with the compile flag : **DCMAKE_BUILD_TYPE=PROFILING** and ran into issues such as: 
```
 [ 9%] Building NVCC (Device) object src/apps/CMakeFiles/relion_gpu_util.dir/__/acc/cuda/relion_gpu_util_generated_cuda_benchmark_utils.cu.o
/var/tmp/assman_g/relion-4.0.1-profile/src/src/acc/acc_ml_optimiser_impl.h(2899): error: expression preceding parentheses of apparent call must have (pointer-to-) function type
       iproj_offset = (op.part_id % 2) * baseMLO->mymodel.nr_classes
                                         ^

1 error detected in the compilation of "/var/tmp/assman_g/relion-4.0.1-profile/src/src/acc/cuda/cuda_ml_optimiser.cu".
CMake Error at relion_gpu_util_generated_cuda_ml_optimiser.cu.o.PROFILING.cmake:282 (message):
  Error generating file
  /var/tmp/assman_g/relion-4.0.1-profile/build/src/apps/CMakeFiles/relion_gpu_util.dir/__/acc/cuda/./relion_gpu_util_generated_cuda_ml_optimiser.cu.o

make[2]: *** [src/apps/CMakeFiles/relion_gpu_util.dir/__/acc/cuda/relion_gpu_util_generated_cuda_ml_optimiser.cu.o] Error 1
make[2]: *** Waiting for unfinished jobs....
```

I therefore had a look into the code and found a semicolon missing in line **2899**  in **/src/acc/acc_ml_optimiser_impl.h**. I added the semicolon and could install a PROFILE version of relion. This is probably relevant for all versions. 

Hope, that helps, 
Greta

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6abb7dbab0ca`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.