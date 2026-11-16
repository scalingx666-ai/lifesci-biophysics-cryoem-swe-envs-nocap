# Issue

**Title:** Address threaded zernike cache and groupUsed race conditions

## Description

Tracking down sporadic crashes when running CTF refinement, I think I located a 2 thread-safety/race conditions. These crashes are quite rare, but occur often enough to motivate me to look into it.

I think the most common cause of what I was seeing is the R_coeffs cache vector in Zernike.cpp . If multiple threads try to populate or grow it at once (like when the cache is totally cold), I think the subsequent assignment back to R_coeffs is not thread safe and bad things can ensue. Possibly also one thread could be reading while another is still assigning back. CTF refinement indeed calls this in multithreaded sections via (for example) AberrationEstimator::parametricFit -> TiltHelper::fitEvenZernike -> computeEvenZernike -> Zernike::Z -> Zernike::R

You could avoid a lock like what I've implemented here by instead making every application precompute the R_coeffs table upfront (it would have to know its maximum `n`) before any multithreaded work, but the code in Zernike.cpp is used in several places and I was nervous to make that change. I didn't find that this change affected performance noticeably, but all I've tested is single particle CTF refinement because that's where I could reproduce crashes, as well as a handful of single particle Refine3D jobs as a quick check.

---

even more rare, I think, was an issue where modifications to a `std::vector<bool>` specifically are not guaranteed thread safe (I'm pretty sure?), so it's more robust as char and doesn't risk corrupting the assignment of whether optics group are occupied/used.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `31c220e3880e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.