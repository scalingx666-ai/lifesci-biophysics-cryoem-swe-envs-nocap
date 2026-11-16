# Issue

**Title:** Add recursive copy for structures

## Description

Certain methods, like `applytransform!`, exist only in mutating form.
Rather than adding a non-mutating version for every potential mutating
function, we can simply copy the structure, apply the transformation,
and return the new structure.

It's worth thinking about whether we like a recursive `copy`. For things like arrays, `copy` is non-recursive: it copies the outer container but any inner containers are referenced identically. There is, of course, `deepcopy`, but that's glacially slow and very unfriendly to the upcoming (Julia 1.12+) static compilation. It seems like we need some form of fast recursive copy, but whether this is called `Base.copy` or `recursivecopy` is a debatable point. Here I went with the former but will be happy to change it to something else.

But for a tree structure (which is what these are), copying only the root node seems silly. So I think this could easily be `Base.copy`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `53d9c1abe157`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.