# Issue

**Title:** Bugfix: Zero-initialize padded posq buffer in AmoebaCommonKernels

## Description

## Description

This pull request fixes a critical uninitialized memory bug in AmoebaCommonKernels.cpp that can cause simulations involving `AmoebaMultipoleForce` and `CustomNonbondedForce` to fail with NaN energies and forces.

The issue is subtle and non-deterministic. It typically only manifests on the first execution of a simulation step within a newly created context (e.g., the first run in a newly launched container or machine). Subsequent runs in the same hardware often succeed because the uninitialized memory may have been overwritten by other operations, masking the bug. This made the issue very difficult to track down.

## Root Cause Analysis

When a system contains `AmoebaMultipoleForce` and the number of particles is not an exact multiple of the compute tile size (typically 32), the `posq` buffer is padded. We discovered that the padded portion of this buffer was not being initialized.

The root cause is located in `CommonCalcAmoebaMultipoleForceKernel::initialize` within [the file](https://github.com/openmm/openmm/blob/master/plugins/amoeba/platforms/common/src/AmoebaCommonKernels.cpp#L223).

1. A temporary host-side vector, `temp`, is created with the full padded size of the `posq` buffer, This vector is allocated on the stack but its contents are uninitialized.
```c++
// The system has numMultipoles particles, but posq is padded.
// e.g., numMultipoles=14, posq.getSize()=32
ArrayInterface& posq = cc.getPosq();
vector<mm_double4> temp(posq.getSize());
```
2. A subsequent loop only populates the data for the actual number of particles (`numMultipoles`), leaving the padded portion of the `temp` vector untouched.
```c++
// This loop only runs from i = 0 to 13.
// Elements temp[14] through temp[31] remain uninitialized garbage.
for (int i = 0; i < numMultipoles; i++) {
    if (cc.getUseDoublePrecision())
        posqd[i] = mm_double4(0, 0, 0, charge);
    else
        posqf[i] = mm_float4(0, 0, 0, (float) charge);
}
```
3. The entire `temp` vector, including the uninitialized garbage in the padded region, is then uploaded to the GPU's `posq` buffer.
```c++
posq.upload(&temp[0]);
```

This constitutes an Undefined Behavior (UB), which explains why the bug appeared sporadically. In our case, we have `AmoebaMultipoleForce` and `CustomNonbondedForce` together in our system, when `CudaNonbondedUtilities::computeInteractions` start executing its [kernels](https://github.com/openmm/openmm/blob/master/platforms/cuda/src/kernels/nonbonded.cu#L157), it calculate force using `force.x -= delta.x*dEdR`, and if the atom index is padded, the `dEdR` is always zero. But once we have a certain `posq2` to be NaN, as IEEE required, a NaN times a ZERO always give a NaN, the `force` afterwards will go broken.

here is the pseudocode.
```c++
                float3 delta = make_float3(posq2.x-posq1.x, posq2.y-posq1.y, posq2.z-posq1.z);
                real dEdR = 0.0f;

                bool isExcluded = (atom1 >= NUM_ATOMS || atom2 >= NUM_ATOMS || !(excl & 0x1));

                COMPUTE_INTERACTION // modify dEdR only if (!isExcluded) 

                force.x -= delta.x*dEdR;
                force.y -= delta.y*dEdR;
                force.z -= delta.z*dEdR;
```

Note `NaN` values would propagate through the force and energy calculations, e.g. `delta, force` will be NaN as shown above, leading to a total potential energy of NaN.

## The Fix
The solution is to ensure the entire `temp` vector is zero-initialized upon creation. I found the [code afterwards we set `posqd` or `posqf`](https://github.com/openmm/openmm/blob/master/plugins/amoeba/platforms/common/src/AmoebaCommonKernels.cpp#L258) does zero-initialized for other buffer, so I will put those logic here.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0ad623417f70`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.