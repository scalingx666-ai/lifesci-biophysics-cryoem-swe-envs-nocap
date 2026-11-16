# Issue

**Title:** Update polymer.py to add clarity to the docs for persistent length

## Description

Changes made in this Pull Request:
Adds
```
.. math::

   C(n) = \langle \cos\theta_{i, i+n} \rangle =
           \langle \mathbf{a_i} \cdot \mathbf{a_{i+n}} \rangle
           
where :math:`\mathbf{a}_i \text{ and } \mathbf{a}_{i+n}` are unit vectors
along the bonds. (Because for two vectors a and b, 
:math:`\mathbf{a} \cdot \mathbf{b} = |\mathbf{a}| |\mathbf{b}| \cos{\alpha}`)
– see 
`Wikipedia <https://en.wikipedia.org/wiki/Dot_product#Geometric_definition>`_.
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
📚 Documentation preview 📚: https://mdanalysis--4901.org.readthedocs.build/en/4901/

<!-- readthedocs-preview mdanalysis end -->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f2efb8c8f374`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.