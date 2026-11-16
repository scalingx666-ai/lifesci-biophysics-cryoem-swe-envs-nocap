# Issue

**Title:** Fix #4731

## Description

Doc fix for self_distance_array()

 

Changes made in this Pull Request:
- Added the increment after the update

```diff python
 for i in range(n):
    for j in range(i + 1, n):
-        k += 1
-        dist[i, j] = d[k]
+       dist[i, j] = dist[j, i] = d[k]
+       k += 1
```

PR Checklist
------------
 - [ ] Tests?
 - [x] Docs?
 - [ ] CHANGELOG updated?
 - [x] Issue raised/referenced?

## Developers certificate of origin
- [x] I certify that this contribution is covered by the LGPLv2.1+ license as defined in our [LICENSE](https://github.com/MDAnalysis/mdanalysis/blob/develop/LICENSE) and adheres to the [**Developer Certificate of Origin**](https://developercertificate.org/).


<!-- readthedocs-preview mdanalysis start -->
----
📚 Documentation preview 📚: https://mdanalysis--4837.org.readthedocs.build/en/4837/

<!-- readthedocs-preview mdanalysis end -->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7f686ca0daa8`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.