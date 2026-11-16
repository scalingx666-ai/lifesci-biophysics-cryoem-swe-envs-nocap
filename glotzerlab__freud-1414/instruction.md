# Issue

**Title:** Minor performance boost for `msd`

## Description

<!-- Provide a general summary of your changes in the Title above. -->

## Description

Current MSD code with mode=="direct" computes a euclidean norm and then squares it. This is now ~2x faster by using an einsum.

## Motivation and Context
```diff
        elif self.mode == "direct":
+           diff = positions - positions[[0], :, :]
            self._particle_msd.append(
-                np.linalg.norm(positions - positions[[0], :, :], axis=-1) ** 2
+                np.einsum("ijk, ijk -> ij", diff, diff)  # Dot product along last axis
            )
```

```

<> : %timeit -r 10 -n 10 np.einsum("ijk, ijk->ij", diff, diff)
5.8 ms ± 910 μs per loop (mean ± std. dev. of 10 runs, 10 loops each)

<> : %timeit -r 10 -n 10 np.linalg.norm(diff, axis=-1)**2
12.1 ms ± 1.13 ms per loop (mean ± std. dev. of 10 runs, 10 loops each)

```

## How Has This Been Tested?

The two expressions are equivalent. 

## Checklist:
- [x] I have reviewed the [**Contributor Guidelines**](https://github.com/glotzerlab/freud/blob/main/CONTRIBUTING.md).
- [x] I agree with the terms of the [**Freud Contributor Agreement**](https://github.com/glotzerlab/freud/blob/main/ContributorAgreement.md).
- [x] My name is on the [list of contributors](https://github.com/glotzerlab/freud/blob/main/doc/source/reference/credits.rst) in the pull request source branch.
- [x] I have updated the [Change log](https://github.com/glotzerlab/freud/blob/main/ChangeLog.md).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `93b4ca10c4ed`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.