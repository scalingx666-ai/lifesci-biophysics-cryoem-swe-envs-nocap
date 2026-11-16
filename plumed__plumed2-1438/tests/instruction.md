# Issue

**Title:** RMSD: reformulate two loops so their omp simd pragmas can be honoured

## Description

##### Description

`./configure && make`, with no arguments at all, fails on any machine whose default C++ compiler is
clang. `configure` auto-detects OpenMP, and `src/tools/Makefile` appends `-Werror`, so the
vectorizer's refusal to honour a `#pragma omp simd` is fatal:

```
RMSD.cpp:1443:35: error: loop not vectorized: the optimizer was unable to perform the requested
transformation; the transformation might be disabled or specified as part of an unsupported
transformation ordering [-Werror,-Wpass-failed=transform-warning]
RMSD.cpp:1487:35: error: (the same, in getDDistanceDReference)
```

Both are attributed to the opening line of the enclosing function, which hides the loop at fault.
Each of the two functions contains exactly one `#pragma omp simd`, so the attribution is
unambiguous: the loops at lines 1479 and 1530, which are the same loop written twice.

**The pragma is kept.** Rather than deleting it, the loop body is rewritten so the compiler can act
on it: the loop-invariant `(ddist_dc*-csum)` is hoisted into a named `Vector`, and the `Vector`
expression is expanded into its three components. Both loops now vectorize at width 4 — confirmed
with `-Rpass=loop-vectorize` — instead of being refused.

I measured the alternatives rather than guessing, all with clang 20.1.1 on the same tree:

| variant | result |
|:---|:---|
| unchanged | ✗ 2 errors |
| braces around the `if` body | ✗ still fails |
| hoisting the invariant only | ✗ still fails |
| binding `Vector& dv=derivatives[iat]` in the body | ✗ still fails |
| flatten to `double*` + manual 3-way unroll | ✓ vectorizes, but the pointer is UB when `n==0` |
| per-component `derivatives[iat][k]` | ✓ vectorizes — **chosen** |

The `Vector` arithmetic creates temporaries the vectorizer cannot see through, and naming the
element through a reference reintroduces them; only the direct per-component form works. Hoisting
alone is not sufficient, but it is necessary, so both changes are needed together.

No other pragma in the file is touched. The other eight active ones already vectorize — their
enclosing functions produce no remark. Two more, on identically shaped `std::vector<Vector>` loops
at lines 1339 and 1426, were commented out long ago with the note that they *"lead to incorrect
results with INTEL compiler"*; the two fixed here had exactly that shape, so this also removes a
latent instance of that hazard.

##### Why CI did not catch it

No job builds clang with OpenMP enabled. `linuxWF.yml` uses gcc and `icpx`; `macWF.yml`'s
`macsimple` job uses AppleClang without `libomp`, so `#pragma omp simd` is inert there and
`-Wno-unknown-pragmas` hides the rest. gcc has no `-Wpass-failed` diagnostic at all. The default
build on a clang host is the configuration nobody exercises.

This is the same class of problem as #1437, found by scanning the whole tree for it afterwards.
`omp simd` occurs in only two files in the entire repository — `src/colvar/PathMSDBase.cpp`, fixed
by #1437, and `src/tools/RMSD.cpp` — and there are no `#pragma unroll`, `#pragma clang loop`,
`ivdep` or `vector` directives anywhere. With this change, an `--enable-modules=all` build is free
of `-Wpass-failed` entirely.

##### Target release

I would like my code to appear in release **2.11** (master).

`v2.10` carries the same two pragmas but *not* the `-Werror` in `src/tools/Makefile`, which arrived
on master only. There it is a warning rather than a build failure, so this is not a forward-merge
candidate unless you would like the warning silenced there too.

##### Type of contribution
- [x] changes to code or doc authored by PLUMED developers
- [ ] changes to a module not authored by you
- [ ] new module contribution or edit of a module authored by you

##### Copyright
- [x] I agree to transfer the copyright of the code I have written to the PLUMED developers or to
      the author of the code I am modifying.
- [ ] the code I am contributing is not mine and I am making it available under LGPL

##### Tests
- [x] I added a new regtest or modified an existing regtest to validate my changes.
- [x] I verified that all regtests are passed successfully on GitHub Actions.

No new regtest is added: this changes neither behaviour nor interface, so there is nothing new to
assert. The existing tests are the check, and they already encode the expected colvars and
derivatives.

Locally I ran the **full** suite twice against the same tree, once with the change and once with
`src/tools/RMSD.cpp` reverted to master, so the comparison isolates this file:

```
                                tests run    failing tests
master, RMSD.cpp reverted           824           20
this branch                         824           20     <- identical set
```

The failure lists are byte-identical, so this change introduces none and hides none. All 20 are
pre-existing and unrelated: 4 are `basic/rt-make-*`, which compile against an *installed* plumed
(this tree was never `make install`ed), and 16 are `ves/rt-bf-*` basis-function tests. The run
enables every module (`--enable-modules=all`) with `--disable-mpi`, which is why the count is
non-zero at all.

The 17 regtests that exercise these code paths all pass, including `rt-rmsd-displace` — the
`align != displace` case, which is the branch the modified loops actually sit in — and
`rt65-rmsd2`, `rt-close-structure` and `rt64-pca`, the three named in the Intel comment above.

Builds verified:

* plain `./configure && make` (clang auto-detected, OpenMP auto-enabled, default modules) — fails
  before this change, clean after: 380 files, 0 errors.
* `--enable-modules=all` with clang + OpenMP + `-O3 -march=skylake-avx512` — clean, 0 `-Wpass-failed`
  across all 56 modules.
* `make -C astyle` leaves the file unchanged.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e44cef66a82f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.