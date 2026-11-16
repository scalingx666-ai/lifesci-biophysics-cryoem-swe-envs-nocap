# Issue

**Title:** Match hybrid pair styles by prefix so an active suffix does not break sub-style checks

## Description

**Summary**

Three call sites compared `force->pair_style` against exact strings (`"hybrid"`, `"hybrid/overlay"`) to detect whether the enclosing pair style is a hybrid variant. When a global suffix is active (e.g., `-sf omp`), `force->pair_style` is stored as `"hybrid/omp"` or `"hybrid/overlay/omp"`, so the exact-match tests fail and the checks are silently skipped. This patch replaces the three equality tests with prefix matches so that hybrid detection is suffix-agnostic.

Affected call sites:
- `src/INTERLAYER/pair_kolmogorov_crespi_z.cpp` — rejects non-hybrid enclosing style; was broken under `-sf omp`
- `src/FEP/compute_fep.cpp` — skips sub-style range validation outside hybrid; was skipped when suffix active
- `src/FEP/fix_adapt_fep.cpp` — same as above

**Related Issue(s)**

None (no open issue exists for this specific bug).

**Author(s)**

Chun-Yeol You, Department of Physics and Chemistry, DGIST (Daegu Gyeongbuk Institute of Science and Technology), Daegu, Republic of Korea. Contact: cyyou@dgist.ac.kr (institutional) / mirryou@gmail.com (long-lived)

**Licensing**

By submitting this pull request, I agree, that my contribution will be included in LAMMPS and redistributed under either the GNU General Public License version 2 (GPL v2) or the GNU Lesser General Public License version 2.1 (LGPL v2.1).

**Artificial Intelligence (AI) Tools Usage**

The fix was developed with the assistance of Claude Code (Anthropic). The AI tool was used to locate the affected call sites and draft the three one-line replacements (`strncmp`-based prefix checks). The author independently verified the root cause, reviewed all generated diffs, and confirmed correctness.

**Backward Compatibility**

No backward-compatibility impact. The three changed lines only widen the matching condition (prefix instead of exact match); any input that worked before continues to work. Inputs that were silently misbehaving under `-sf omp` will now behave correctly.

**Implementation Notes**

`hybrid`, `hybrid/overlay`, and `hybrid/scaled` are each registered under two style names — the canonical name and a `/omp` alias that resolves to the same class:

```
PairStyle(hybrid,PairHybrid);
PairStyle(hybrid/omp,PairHybrid);
PairStyle(hybrid/overlay,PairHybridOverlay);
PairStyle(hybrid/overlay/omp,PairHybridOverlay);
```

With a global suffix active, `Force::store_style()` records e.g. `"hybrid/overlay/omp"` in `force->pair_style`. A prefix match with `strncmp(force->pair_style,"hybrid",6)==0` (resp. `"hybrid/overlay"`, length 14) correctly handles both the plain and suffixed variants without hardcoding every possible suffix.

Correctness was verified by manual inspection; the change is a strict relaxation of the existing guard condition and does not affect any logic path that was previously reachable.

**Post Submission Checklist**

- [x] The feature or features in this pull request is complete
- [x] Licensing information is complete
- [x] Corresponding author information is complete
- [ ] The source code follows the LAMMPS formatting guidelines
- [ ] Suitable new documentation files and/or updates to the existing docs are included
- [ ] The added/updated documentation is integrated and tested with the documentation build system
- [ ] The feature has been verified to work with the conventional build system
- [ ] The feature has been verified to work with the CMake based build system
- [ ] Suitable tests have been added to the unittest tree.

**Further Information, Files, and Links**

This bug was discovered while running LAMMPS with `-sf omp` on KOLMOGOROV/CRESPI and FEP pair styles. The root cause (`force->pair_style` storing the suffixed name) was confirmed by reading `src/force.cpp:store_style()`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c4fe7a5bcf91`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.