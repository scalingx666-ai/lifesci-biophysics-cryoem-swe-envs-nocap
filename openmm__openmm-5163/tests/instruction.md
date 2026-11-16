# Issue

**Title:** Fix uninitialized buffers in QTB

## Description

`adaptFrictionPart1` is executed first, it reads `randomForce` and `segmentVelocity` buffers before other kernels write them so they can contain garbage after allocation (cuMemAlloc/hipMalloc/clCreateBuffer do not clear allocated memory).

The errors are observed on HIP and OpenCL in `testRandomSeed` of TestQTBIntegrator:
```
 56/195 Test #342: TestHipQTBIntegratorSingle ....................***Failed   26.18 sec
exception: Assertion failure at TestQTBIntegrator.h:393.  Expected 1.94472, found 1.9414

176/195 Test #343: TestHipQTBIntegratorMixed .....................***Failed   26.40 sec
exception: Assertion failure at TestQTBIntegrator.h:393.  Expected 1.88941, found 1.94636

162/195 Test #344: TestHipQTBIntegratorDouble ....................***Failed   25.85 sec
exception: Assertion failure at TestQTBIntegrator.h:393.  Expected 1.9588, found 1.94636
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `adfd84c27330`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.