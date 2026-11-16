# Issue

**Title:** Extended XYZ files with both force and forces

## Description

A file written by one program with `forces` and given `force` by another (here a training set with `Properties=species:S:1:pos:R:3:initial_charges:R:1:initial_magmoms:R:1:force:R:3:forces:R:3`, 3105 frames) gave `property 'forces' appears twice`: the first property that gives a known column took it, so `force` took `forces`, and `forces` then kept its own name, `forces`, again.

Now a property named as its column takes it, in either order; otherwise the first property that gives a known column does, as before (`vel` before `momenta`); the others keep their own names. Only two properties of the very same name are an error. The same holds for `charge` beside `initial_charges`.

- Tests: both orders of `force` and `forces`, `charge` beside `initial_charges`, and a round trip; the error case now uses `forces` twice. All io tests pass (853).
- The file reads whole in 0.4 s and opens in the application.
- The format page and the release notes of 2.0.0 say so.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ea82ba8fd339`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.