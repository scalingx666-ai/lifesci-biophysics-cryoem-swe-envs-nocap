# Issue

**Title:** Add option to evaluate only reference contact pairs

## Description

Changes made in this Pull Request:

`Contacts` builds the full distance matrix of the two selections on every frame and then keeps only the entries that were in contact in the reference. When the native contacts are a tiny fraction of all pairs (for example all protein heavy atoms), nearly all of that work gets thrown away.

This PR adds an opt-in `compute_pairs_only=False` keyword. With `compute_pairs_only=True`, the contact indices and reference distances are worked out once per run, and each frame calls `calc_bonds` on just those pairs, using the same box and the same `method`. The default code path is untouched, and the results match the default (compared at `rtol=1e-12` in the tests and in the benchmark below).

In short, instead of computing every distance and discarding most, we remember once which pairs matter and compute only those.

**Benchmark.** 15 cases on 7 systems (macOS ARM64, serial, `OMP_NUM_THREADS=1`, median of 3–5 alternating runs, `Contacts.run()` only). "Density" is the share of all N×N atom pairs that are reference contacts. "init+run" also counts building the `Contacts` object, whose dense reference setup is unchanged. "Memory" is the peak memory allocated during `run()` (`tracemalloc`).

| System | Selection | Atoms | Frames | Density | `compute_pairs_only=False` | `compute_pairs_only=True` | Speedup (run) | Speedup (init+run) | Memory in `run()`, MiB (`compute_pairs_only=False` / `True`) |
|---|---|---:|---:|---:|---:|---:|---:|---:|---|
| AdK equilibrium | first 64 heavy atoms, radius 1000 Å (every pair is a contact) | 64 | 256 | 100% | 0.021 s | 0.041 s | **0.52× (slower)** | 0.52× | 0.2 / 0.3 |
| AdK equilibrium | salt bridge (Asp/Glu vs Arg/Lys) | 70 | 256 | 1.1% | 0.015 s | 0.014 s | 1.1× | 1.1× | 0.2 / 0.2 |
| AdK equilibrium | CA, 8 Å | 214 | 256 | 4.6% | 0.039 s | 0.027 s | 1.5× | 1.5× | 0.5 / 0.2 |
| Small protein, test files (TPR/TRR) | heavy atoms | 480 | 3 | 3.3% | 0.021 s | 0.020 s | 1.05× | 1.05× | 2.0 / 0.4 |
| [I-FABP](https://www.mdanalysis.org/MDAnalysisData/ifabp_water.html) | heavy atoms | 1057 | 256 | 1.5% | 0.448 s | 0.135 s | 3.3× | 3.3× | 9.2 / 1.1 |
| AdK, [test files](https://github.com/MDAnalysis/mdanalysis/tree/develop/testsuite/MDAnalysisTests/data) (PSF/DCD) | heavy atoms | 1656 | 98 | 1.0% | 0.521 s | 0.084 s | 6.2× | 6.0× | 21.8 / 1.7 |
| AdK in water, test files (TPR/XTC) | heavy atoms | 1656 | 10 | 1.0% | 0.641 s | 0.037 s | 17.4× | 7.1× | 21.8 / 1.6 |
| [AdK equilibrium](https://www.mdanalysis.org/MDAnalysisData/adk_equilibrium.html) | heavy atoms | 1656 | 256 | 1.0% | 1.655 s | 0.202 s | 8.2× | 8.0× | 21.8 / 1.6 |
| AdK equilibrium | heavy atoms, 8 Å cutoff | 1656×1656 | 32 | 4.4% | 0.249 s | 0.115 s | 2.17× | 2.10× | 24.7 / 7.0 |
| AdK equilibrium | heavy atoms, 12 Å cutoff | 1656×1656 | 32 | 12% | 0.347 s | 0.337 s | 1.03× | 1.03× | 31.1 / 19.0 |
| AdK equilibrium | heavy atoms, 20 Å cutoff | 1656×1656 | 32 | 39% | 0.520 s | 0.998 s | 0.52× | 0.52× | 53.4 / 60.9 |
| Membrane protein, test files (`GRO_MEMPROT`) | heavy atoms | 4372 | 5 | 0.4% | 2.120 s | 0.082 s | 25.9× | 5.1× | 148.1 / 4.4 |
| [NhaA](https://www.mdanalysis.org/MDAnalysisData/nhaa_equilibrium.html) | heavy atoms | 5622 | 64 | 0.3% | 4.944 s | 0.360 s | 13.7× | 11.2× | 244.1 / 5.7 |
| Membrane protein, test files (`GRO_MEMPROT`) | all heavy atoms incl. lipids | 18062 | 5 | 0.08% | 37.265 s | 0.894 s | 41.7× | 5.1× | 2496.6 / 14.7 |
| NhaA | lipid heavy atoms vs protein heavy atoms | 19266×5622 | 16 | 0.0039% | 3.866 s | 0.299 s | 12.92× | 7.23× | 826.7 / 0.5 |

So it helps a lot when the contacts are sparse and the selection is large, is neutral for small selections, and is slower when almost every pair is a contact. That is why it is opt-in rather than the default. The Asp/Glu–Arg/Lys case is also a small selection, so it doesn't gain much either. 

**Test:** `pytest testsuite/MDAnalysisTests/analysis/test_contacts.py`. The existing tests are parametrized over `compute_pairs_only` and run with both `False` and `True` for hard/soft/radius cut, with and without PBC (serial and multiprocessing backends), and with a custom method. New tests cover several overlapping references and rerunning after editing the mask.


## LLM / AI generated code disclosure
LLMs or other AI-powered tools (beyond simple IDE use cases) were used in this contribution: yes
patch was checked with LLMs for quality, tests done with LLM and benchmark script were drafted with an AI coding assistant. 

## PR Checklist
 - [x] Issue raised/referenced? Yes #5517
 - [x] Tests updated/added?
 - [x] Documentation updated/added?
 - [x] `package/CHANGELOG` file updated?
 - [x] Is your name in `package/AUTHORS`? (If it is not, add it!)
 - [x] I have read and understand the current [AI Policy](https://github.com/MDAnalysis/mdanalysis/blob/develop/AI_POLICY.md)
 - [x] LLM/AI disclosure was updated.

## Developers Certificate of Origin
I certify that I can submit this code contribution as described in the [**Developer Certificate of Origin**](https://developercertificate.org/), under the MDAnalysis [LICENSE](https://github.com/MDAnalysis/mdanalysis/blob/develop/LICENSE).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `607a5a9eac4d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.