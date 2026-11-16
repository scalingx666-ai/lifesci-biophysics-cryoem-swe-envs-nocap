# Issue

**Title:** Updated code with inclusive terms leader and follower

## Description

Hello RELION developers - 

Related to discussions of how to make science and cryoEM inclusive, we should consider removing 'master' and 'slave' from our code. To help facilitate this, I'm submitting this pull request where I replaced 'master' and 'slave' with 'leader' and 'follower,' respectively.

Some additional materials discussing this situation in the life sciences & computing:

- [Equity, Diversity and Inclusion: A call to eradicate non-inclusive terms from the life sciences](https://elifesciences.org/articles/65604)
- [Tech Confronts Its Use of the Labels ‘Master’ and ‘Slave’](https://www.wired.com/story/tech-confronts-use-labels-master-slave/)

I've tested the changes on my end without any issues in analyzing the tutorial dataset. 

If you prefer alternative terms I can also change and submit a different pull request. 

Thank you!
Mike

<img width="549" alt="Screen Shot 2021-02-23 at 12 27 50 PM" src="https://user-images.githubusercontent.com/1464469/108883928-01e16f80-75d4-11eb-9fad-f581de5dda08.png">

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `da0b41cb1d13`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.