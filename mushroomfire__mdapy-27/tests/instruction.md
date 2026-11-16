# Issue

**Title:** GPU pictures: no lens in the shaders of Metal and Vulkan

## Description

`render` lost its depth of field in #23, but the GPU shaders still sampled a lens whenever `aperture > 0`, which nothing sets any longer.

- The lens is gone from the shaders of Metal and Vulkan; the SPIR-V is compiled again with glslc of shaderc 2026.4.
- `aperture` and `focal_distance` stay in `gpu::Params` and in the shaders' declarations of it: the shader of Direct3D 12 shares that layout and is compiled by hand with dxc on Windows. They go from all three together when it is compiled again.
- The lens drew random numbers only when it was used, so pictures are unchanged: rendered before and after, four scenes (perspective with several samples, soft shadows, orthographic) are identical to the last bit on Metal (Apple M5) and on Vulkan (NVIDIA RTX A2000, with its ray tracing units). The tests of pictures pass on both.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `104e15a2618d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.