# Issue

**Title:** Add samplers hooks to YAML scripts

## Description

This adds the sampler hooks into the YAML syntax as discussed in #898 . I've also added the `default_number_of_iterations` global option.

@Lnaden, I still have to update the `whatsnew` and the YAML docs, but I don't think I'll have time to do it before Tue (I need to do another couple of things before the end of the day) so feel free to merge/modify this if you need it before next week.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `445dc4212871`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.