# Issue

**Title:** make rng private for selectors

## Description

(partly)

The notebook from that issue now works. However, It is now uncertain is the state of the generators that is loaded. 
This depends whether OPS is re-imported between loading or not.

```python
# before simulation:
{'bit_generator': 'PCG64',
 'state': {'state': 336467696170254096309584224979479325155,
  'inc': 238122374084886836437703854582102603205},
 'has_uint32': 0,
 'uinteger': 0}
<numpy.random._pcg64.PCG64 at 0x7f0b819d9d10>
 
# after running:
 {'bit_generator': 'PCG64',
 'state': {'state': 195230481313007235580189205770362297672,
  'inc': 238122374084886836437703854582102603205},
 'has_uint32': 1,
 'uinteger': 2214207804}
 <numpy.random._pcg64.PCG64 at 0x7f0b819d9d10>
 
# loaded in the same kernel session
{'bit_generator': 'PCG64',
 'state': {'state': 195230481313007235580189205770362297672,
  'inc': 238122374084886836437703854582102603205},
 'has_uint32': 1,
 'uinteger': 2214207804}
<numpy.random._pcg64.PCG64 at 0x7f0b819d9d10>
 
# loaded in a restarted kernel session
 {'bit_generator': 'PCG64',
 'state': {'state': 316665816032858718566197330052104521012,
  'inc': 90421417105856406385200004491830534511},
 'has_uint32': 0,
 'uinteger': 0}
 <numpy.random._pcg64.PCG64 at 0x7f0ab8347d10>
```
This is a broader issue to be solved, as OPS does not have a way of storing mutable objects (such as a rng state)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3c5211d2735a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.