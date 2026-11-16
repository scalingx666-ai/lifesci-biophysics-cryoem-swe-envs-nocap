# Issue

**Title:** fix(dp): handle compact canonical artifacts

## Description

**Summary**

Prevent compact canonical DeepMD artifacts, including compressed DPA4C, from being sent to the raw device-edge API that cannot serve them.

**Modification**

Gate GPUMD's `compute_gpu_edges` fast path with `!deep_pot.uses_canonical_graph_inference()`. Compact canonical artifacts now use the existing standalone `DeepPot::compute` path, which constructs and evaluates their canonical graph internally. Edge/vector and ordinary graph artifacts continue to use the existing GPU-resident edge path. The change is confined to `src/force/dp.cu`.

DeepMD intentionally reports `supports_device_edge_inference() == true` for edge, graph, and compact canonical artifacts. That broad capability means the artifact has a device inference ABI; it is not permission to call `compute_edges_gpu()` for every lower kind. DeepMD rejects that call for a compact canonical artifact and requires `compute_canonical_graph_gpu()` or the standalone compute entry point.

**Validation**

Validated on an NVIDIA A800-SXM4-80GB with DeePMD-kit 3.2.0, PyTorch 2.11.0+cu126 and a freshly generated compressed DPA4C artifact. The artifact was produced by `dp --pt-expt freeze --lower-kind graph` followed by `dp --pt-expt compress`; its embedded metadata is `lower_input_kind=dpa4c_canonical`, `graph_edge_dtype=float32`, size 7,595,038 bytes and SHA-256 `f297b94d1cb508560103dac81e10ec9043992741502739ef5db868695528ac35`.

The same artifact and 1,536-atom triclinic water configuration were evaluated through DeepMD Python, LAMMPS Kokkos, pre-fix GPUMD master and the fixed GPUMD build (Volc task `t-20260826002014-nps4b`).

- Pre-fix master reproduced the reported error exactly: `compute_edges_gpu cannot serve a compact canonical artifact; use compute_canonical_graph_gpu.`
- DeepMD Python and LAMMPS Kokkos both completed on the same artifact, isolating the defect to GPUMD's call routing rather than DeepMD compression.
- Fixed GPUMD completed. Versus the DeepMD Python result: |dE| = 6.29e-5 eV, max |dF| = 1.43e-5 eV/A, max |dV| = 2.19e-4 eV.
- LAMMPS versus DeepMD Python: |dE| = 3.47e-5 eV, max |dF| = 1.86e-5 eV/A, max |dV| = 1.05e-4 eV.

A broader regression refresh covering DPA4, uncompressed DPA4C and compressed DPA4C (EFV T1/T3, NVE, RDF and efficiency) is being recorded separately; it does not expand this surgical source change.

**Licensing**

By submitting this pull request, I agree that my contribution will be included in GPUMD and redistributed under the existing GPUMD license. No external source code with incompatible licensing has been copied into this pull request.

**Others**

This is intentionally a minimal correctness fix. Native construction of the compact canonical graph in GPUMD can be considered separately if a fully GPU-resident path is desired.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `36c6d5e05adb`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.