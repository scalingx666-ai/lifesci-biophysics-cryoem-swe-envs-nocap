# Issue

**Title:** Pictures from far away without rings

## Description

Pictures of large systems seen from far away showed rings centered on the middle of the picture, dark on the GPU and fainter on the processor: a polycrystal of a million atoms framed whole, the camera some 1000 Å from it, has them plainly.

Two losses of precision in single precision, both growing with the distance of the camera:

- **The intersections.** A ray met a sphere where `b² - c = 0`, with `b = (o - c)·d` and `c = |o - c|² - r²`: from far away both terms are near 10⁶ while their difference is near `r²`, so the difference kept few digits, and which hits it lost varied smoothly with the angle of the ray, in rings. The discriminant is now `r² - |o - c - b d|²`, the radius less the squared distance of the ray from the center, which is exact to the size of the atom however far the ray starts. The bonds, cylinders, have the same form across their axis.
- **The lift off a surface.** The rays of shadows and occlusion left a hit point by a length that depends on the depth of the scene only, while the hit point itself is rounded by about 10⁻⁷ of its distance from the eye: from far away the point fell inside its own atom and the atom shadowed itself, in specks. The lift now grows by 10⁻⁶ of that distance, a hundredth of an ångström from 3000 Å, and stays as it was near the camera.

The processor and the three GPU shaders change together, so that their pictures still differ only by rounding; the SPIR-V is compiled again with glslc of shaderc 2026.4 and the DXIL with dxc 1.7.2308.16. A new test renders an atom and a bond from 3400 Å through a narrow view and compares with the same picture without perspective, on the processor and on the GPU; it fails without either change.

Tested: macOS (Metal) 154 passed; Windows (Direct3D 12 on an RTX 4060) 139 passed with its ray tracing units and 138 without.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `208df75cab76`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.