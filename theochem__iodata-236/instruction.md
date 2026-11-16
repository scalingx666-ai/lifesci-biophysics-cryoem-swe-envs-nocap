# Issue

**Title:** Improve Input Writer docs & allow string template.

## Description

As discussed in #234 , we should allow a string input to the input writer. I haven't implemented that yet, I'm waiting for consensus on how we should differentiate between passing a string template and passing a filename, but I'm happy to implement it once we've decided which way to go about it. This PR will close #234 

In the meantime, I added a section to the "Supported Input Formats" page that shows the default template for each format. @tovrstra , feel free to directly fix any mistakes I made with that, but as far as I can tell it works correctly. 

I will also add a small section about the templates in the Getting Started page.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `270997707b66`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.