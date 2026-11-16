# Issue

**Title:** GPU pictures: no lens in the parameters of the shaders

## Description

The depth of field left `render` in 2.0.0b1, but the parameters of the GPU shaders still carried `aperture` and `focal_distance`, always 0 and 1, because the shader of Direct3D 12 still read them. They are gone now from `gpu::Params` and from the Metal, Vulkan and Direct3D 12 shaders, and the Direct3D 12 shader loses its lens. The lens only ran with an aperture, so no picture changes.

The parameters are 236 bytes now; the Direct3D 12 shader reads the scene after them at that offset (`kParams`, and a `static_assert` on the host). The SPIR-V was compiled again with glslc of shaderc 2026.4 and the DXIL with dxc 1.7.2308.16 of the Windows SDK 10.0.26100.0; the tests that compile the shaders from their source agree.

Tested on the three systems:

- macOS, Metal (Apple M5): the tests of `tests/visualization` pass.
- Windows 11, Direct3D 12 on an RTX 4060 Laptop GPU, with its ray tracing units and without (`MDAPY_GPU_RAY_TRACING=0`): 137 and 136 pass.
- Linux (Ubuntu 24.04, GCC, warnings as errors), Vulkan through Mesa's dzn on the same GPU: 135 pass.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3374e69429d3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.